# Create an autograding image

An autograding image is a template for the sandboxed environment that submissions will run in. It defines which operating system will be used and which software is available while running an autograder. It should not contain any project-specific information.

If you're using a common tech stack, there's a good chance you won't need to create a custom image. You can see all the current images in our [Autograding Images repository](https://github.com/UB-CSE-IT/Autograding-Images).

Before creating your own image, it's a good idea to be somewhat familiar with [The autograding process](The%20autograding%20process.md).

## Creating your own image

I'll walk through the process of a minimal Python grading image. You can also review a full example Dockerfile [here](https://github.com/UB-CSE-IT/Autograding-Images/blob/main/dockerfiles/Dockerfile_autograding_image).

Create a [Dockerfile](https://docs.docker.com/reference/dockerfile/). This will contain commands to initially set up the sandboxed environment for grading. It should NOT contain any project-specific details; that is left to your [autograder](Create%20an%20autograder.md).

The first line needs to specify a base image that you're starting from. The latest LTS version of Ubuntu is usually a good choice. Add the `LABEL org.opencontainers.image.authors` line with your name/email address. Formatting conventions are more detailed in the [Autograding Images repository](https://github.com/UB-CSE-IT/Autograding-Images).

```dockerfile
FROM ubuntu:24.04
LABEL org.opencontainers.image.authors="Nicholas Myers"
```

Then, install the necessary [Autodriver](The%20autograding%20process.md#autodriver) by pasting this exact block into your Dockerfile:

```dockerfile
# Install autodriver
WORKDIR /home
RUN useradd autolab
RUN useradd autograde
RUN mkdir autolab autograde output
RUN chown autolab:autolab autolab
RUN chown autolab:autolab output
RUN chown autograde:autograde autograde
RUN apt update
RUN apt install -y sudo git make gcc
RUN git clone https://github.com/autolab/Tango.git
WORKDIR Tango/autodriver
RUN make clean && make
RUN cp autodriver /usr/bin/autodriver
RUN chmod +s /usr/bin/autodriver
RUN apt -y autoremove
RUN rm -rf Tango/
WORKDIR /home
```

Installing the Autodriver early allows Docker to cache the layer to speed up iterative builds as you make modifications.

Next, run any Linux commands necessary to build the environment. Avoid installing more packages than necessary. In this case, I'll just install Python and Pip.

```dockerfile
# Install Python3
RUN apt update
RUN apt install -y python3 python3-pip
```

At the end of the file, you can optionally run some debugging lines to verify software is installed properly. You'll only see this in the build logs. Students won't see any output from your Dockerfile when submitting.

```dockerfile
# Verify
RUN python3 --version
```

**Important**: Your final `WORKDIR` **MUST** be `/home`! If you changed it after installing the Autodriver, add `WORKDIR /home` to the end of the file.

Keep in mind that when a submission runs, the four files (your Makefile, your autograder, the submission, and the submission metadata) are in /home/autograde/autolab, and Make will be called in that directory as the autograde user. This doesn't affect this demo Dockerfile, but it may matter for more advanced setups. This is further explained in [The autograding process](The%20autograding%20process.md).

## Testing your image

Build your image with the next command. Replace `Dockerfile_cse_123` with your file name and `cse_123` with your image name. It should follow the same naming convention.

```bash
docker build -f Dockerfile_cse_123 -t cse_123 --progress=plain .
```

Run that command any time you update your Dockerfile.

The `--progress=plain` option will show the output of your debugging lines.

You can forcefully rebuild the entire image with the `--no-cache` option if changes don't seem to be applied.

Create a directory that mimics how Tango will arrange the files.

For example: in the `/path/to/directory` directory, I'll place:  `Makefile`, `handin.py`, and `autograde.tar`. You could also add `settings.json` if your grader depends on it. Autolab will always provide these four files. (The file name of the student submission is configured in your [handin settings](Create%20an%20assessment.md#handin). The Makefile and autograde.tar come from your [autograder](Create%20an%20autograder.md).)

To mimic running a job the way Tango will, use the following command. 
* Update `cse_123` to your actual image name.
* Update `/path/to/directory/` to the location you placed the 3 or 4 files.

```bash
docker run --rm -v /path/to/directory/:/home/mount cse_123 sh -c 'cp -r mount/* autolab/; su autolab -c "autodriver -u 100 -f 104857600 -t 20 -o 1024000 autolab > output/feedback 2>&1"; cp output/feedback mount/feedback'
```

This complicated command is explained in depth in [The autograding process](The%20autograding%20process.md#running-the-job). (Though, it's slightly different here for your convenience.)

After running this command, view the autograder output from the `/path/to/directory/feedback` file.

You can re-run the grader after updating the submission. The feedback file will be overwritten on each run.

## Add your image to Autolab

After verifying your image works with your autograder as expected, open a pull request with the image in the [Autograding Images repository](https://github.com/UB-CSE-IT/Autograding-Images). Be sure to follow the formatting instructions in the README file.