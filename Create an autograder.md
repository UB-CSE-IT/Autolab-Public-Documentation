# Create an Autograder

This is arguably the most important feature of Autolab. An autograder will run automatically after a student makes a
submission to an assessment. As an instructor, you will write code that grades a student's submission. When a student
submits their work, it will be copied into a Docker container, along with your grading files and some metadata. Your
code should interact with the students submission in some way to determine its correctness. You can print feedback to
stdout for the student to see. The final line you print must be a JSON string with the student's scores for each problem
in the assessment.

## Setup

### Create an assessment

Start by creating an assessment as described in the [Create an Assessment](Create%20an%20assessment.md) guide.

From the assessment page, click `Edit assessment`.

![Edit assessment](screenshots/edit_assessment.png)

### Set the handin filename

Choose the handin filename from the "Handin" tab. This is the name of the file that your autograder will
need to interact with. The extension is an important hint about what type of file students should upload. In this
simple grader, I'll have the students upload a Python script.

> ### Have a multi-file project?
> Since students can only submit one file, it's common to request they upload a .zip or .tar.gz archive for larger
> projects. Then, your grader can decompress the archive and have access to all the files.

![Handin py extension](screenshots/handin_py_extension.png)

### Add some problems

From the `Problems` tab, add some problems that will be graded by our autograder.

![Autograder problems](screenshots/autograder_problems.png)

> ### Save your changes
> Remember, Autolab doesn't apply most settings immediately. You'll need to click `Save` at the bottom of the page after
> making these changes.

### Add an autograder

From the `Basic` tab, scroll down to the "Modules Used" section, and click the plus next to "Autograder".

![Autograder plus](screenshots/autograder_plus.png)

1. Choose a VM image to use. This is the Docker image that your container will be built from. The `autograding_image` is
   a good choice for Python projects.
    * You can see all the Dockerfiles for Autolab's VM images in our
      previous [Tango Repository](https://github.com/UBAutograding/Tango/tree/master/vmms). If you need a custom image,
      you can create one and make a pull request to that Tango repository. Then, contact CSE IT to build the image and
      add it to Autolab. Try to use an existing image if possible.
2. Choose the timeout for the autograder. This is the maximum amount of time your autograder will be allowed to run. If
   grading takes longer than this, the student will receive partial feedback and an error saying that Autolab couldn't
   parse the output from your grader.
    * Our new hardware as of fall 2023 is much faster, and many jobs finish in under 10 seconds. I recommend uploading
      your own solution to the assessment and running it to get an idea of how long it takes to run. Then, at your
      discretion, add some time to account for performance variation and inefficient submissions.
    * If you want to grade based on runtime, this is not the way to do it. You'll need to implement the timeout feature
      within your grader. Since there's variation between runs, consider even timing your solution within the same
      container and adjust the expected runtime dynamically.
3. Click `Save Settings`. We'll come back to this page later to upload the grader.

![Autograder initial settings](screenshots/autograder_initial_settings.png)



