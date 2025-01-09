# The autograding process

Autolab follows a complex procedure for running an autograding job. Jobs are executed in sandboxed containers to ensure they can't interfere with shared resources and distributed across multiple servers to handle a large quantity of submissions.

[Tango](https://github.com/UB-CSE-IT/Tango) is Autolab's autograding backend. It handles job queueing and distribution. It's a separate service from Autolab, but they're tightly integrated. Tango's API is [documented by CMU](https://docs.autolabproject.com/tango-rest/).

## Sending the job to Tango

An autograding job is started when a user makes a submission to an assignment with an autograder configured or an instructor regrades an existing submission.

To begin a job, Autolab first makes a GET request to Tango's `/open/<key>/<course-assessment>/` endpoint. Tango creates a directory for the assignment if it doesn't already exist. If the directory already exists, Tango returns the MD5 hash of every file in the directory so Autolab can avoid uploading duplicates (e.g., your autograder files).

Next, Autolab repeatedly makes POST requests to Tango's `/upload/<key>/<course-assessment>/` endpoint for each (new; skipping MD5 collision duplicates) file the job requires.

This includes:

* The [autograder](Create%20an%20autograder.md#autogradetar): `autograde.tar`
* The [Makefile](Create%20an%20autograder.md#makefile): `autograde-Makefile`
* The submission: E.g., `student@buffalo.edu_5_handin.py`
* The submission [metadata](Create%20an%20autograder.md#student-metadata): E.g., `student@buffalo.edu_5_handin.py.settings.json`

Files are saved to `/opt/TangoService/Tango/courselabs/<key>-<course>-<assignment>/<filename>`

After uploading all required files, Autolab generates a random callback URL, which Tango will request when the job is finished.

To queue the job, Autolab makes a POST request to Tango's `/addJob/<key>/<course-assessment>/` endpoint. This request contains the following information:

* The [image name](Create%20an%20autograder.md#add-an-autograder) configured in the autograder settings
* The [timeout](Create%20an%20autograder.md#add-an-autograder) configured in the autograder settings
* Each file required for grading
  * This contains the original filename on Tango's filesystem (e.g., `student@buffalo.edu_5_handin.py`) and the destination filename (e.g., `handin.py`), which is what the file will be named within the container when grading.
* The name of the feedback output file (generated by Autolab, ends with `_autograde.txt`)
* The callback URL mentioned above (generated by Autolab)
* The job name (generated by Autolab)

Tango validates the job (e.g., all required files exist, the image exists, etc.) and returns an error if there is one. Error messages are propagated back to Autolab. Autolab will reveal more detailed errors to instructors, while students will see vague errors.

## Tango queues the job

If the job is valid, Tango adds it to the job queue. Tango has a configured number of container instances that can run simultaneously for each image. The job will remain in the queue until there is an instance of the required image available. Usually, jobs are dequeued immediately, but during high-traffic times, such as assignment deadlines, the queue may get longer.

## Running the job

When a job is ready to run, Tango:

* Picks a worker node to run the job on
* Creates a directory on the worker to copy the job files to via SSH
  * E.g., `/docker-volumes/dev-1005-autograding_image`
* Copies each job file to the worker node via SCP
  * E.g., `/docker-volumes/dev-1005-autograding_image/handin.py`
* Executes the command to begin autograding on the worker node via SSH (elaborated below)

The command that begins autograding is similar to the following. This example is from a development environment, but the core of the command is identical in production.

```bash
(docker run --name dev-1005-autograding_image -v /docker-volumes/dev-1005-autograding_image/:/home/mount autograding_image sh -c 'cp -r mount/* autolab/; su autolab -c "autodriver -u 100 -f 104857600 -t 20 -o 1024000 autolab > output/feedback 2>&1"; cp output/feedback mount/feedback')
```

Let's break that complicated command down:

* It creates and starts a new Docker container from the image you specify (e.g., `autograding_image`)
* The directory with the files required for the job is mounted to `/home/mount` within the container
* Within the container, the following command is executed: `cp -r mount/* autolab/; su autolab -c "autodriver -u 100 -f 104857600 -t 20 -o 1024000 autolab > output/feedback 2>&1"; cp output/feedback mount/feedback`
  * All the input files are copied to the `/home/autolab` directory
    * The final `WORKDIR` in your Dockerfile should always be `/home`. This document assumes it is.
  * It switches to the `autolab` user, which must be created in your Dockerfile. (Up until this point, it has been running as whichever user you specified in your Dockerfile, or usually `root` by default.)
  * The `autolab` user executes `autodriver -u 100 -f 104857600 -t 20 -o 1024000 autolab > output/feedback 2>&1`
    * (These limits are different in production)
    * `-u 100` limits the number of processes that can be started
    * `-f 104857600` sets the maximum file size that can be created
    * `-t 20` sets the job timeout
    * `-o 1024000` limits the size of the output
    * `autolab` specifies the directory that will be copied into the grading user's home directory
    * `> output/feedback 2>&1` saves stdout and stderr to the feedback file
  * (autodriver is explained in depth below)
  * After grading, we're back to the original user, and the feedback file is copied to `/home/mount/feedback`, which is shared with the host

### Autodriver

The [autodriver](https://github.com/UB-CSE-IT/Tango/blob/master/autodriver/autodriver.c) configures the environment in the container before running your autograder.

It does a lot, but the most important parts are:

* It is running as the `autolab` user (but the submission code is NOT; keep reading)
* It moves the files from `/home/autolab` to `/home/autograde/autolab`
* It sets `/home/autograde/autolab` as the CWD
* It changes ownership of the `/home/autograde` directory recursively to the `autograde` user
* It forks a child process to run the instructor's autograder with limited privileges
  * The child process runs as the `autograde` user, but the environment is NOT updated
    * This means that running `whoami` prints `autograde`, but the environment variables are: `USER: autolab` and `HOME: /home/autolab`
  * The child process' stdout and stderr are redirected to `/home/autograde/output.log`
  * The child process calls `Make` to begin running your autograder
* It exits after Make either successfully exits or times out

The main takeaway is that the four files (your Makefile, your autograder, the submission, and the submission metadata) are in `/home/autograde/autolab`, and Make will be called in that directory as the `autograde` user.

### After the job is finished

After a job finishes, Tango:

* Copies the feedback file (e.g., `/docker-volumes/dev-1005-autograding_image/feedback`) from the worker node to its filesystem (e.g., `/opt/TangoService/Tango/courselabs/<key>-<course>-<assignment>/output/student@buffalo.edu_5_assignment_autograde.txt`) via SCP
* Destroys the Docker container and the volume (e.g., `/docker-volumes/dev-1005-autograding_image/`) on the worker node
* Removes the job from the queue and disassociates the worker node from the job container
* Creates a temporary file combining a header (with the job history and status) and the feedback file
* Makes a POST request to Autolab's callback URL with the result file to notify Autolab that the job finished

Autolab assigns the feedback from Tango to the submission (in the database) and saves it to the filesystem.

Autograding is complete!

(And all of this happens in under 3 seconds for minimal autograders!) 