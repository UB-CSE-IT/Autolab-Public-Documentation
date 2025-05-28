# Introduction

Autolab is an open-source automated code grading platform developed at Carnegie Mellon University and used by
universities around the world. The UB CSE IT department maintains a customized variant of Autolab for use in our
courses. It's hosted on premises at UB, and you can access it at <https://autolab.cse.buffalo.edu>.

This documentation will serve as a guide, primarily for instructors, on how to use Autolab. It covers nearly everything
an instructor may be interested in, from the basics to more advanced features. If you have any questions after
consulting the documentation, feel free to [contact us](https://autolab.cse.buffalo.edu/contact).

## Table of Contents

### [Getting Started](Getting started.md)

* [Your First Login](Getting started.md#your-first-login)
* [Create a Course](Getting started.md#create-a-course)

### [Course Management](Course management.md)

* [Viewing Your Courses](Course management.md#viewing-your-courses)
* [Course Page](Course management.md#course-page)
* [Manage Course](Course management.md#manage-course)
* [Course Settings](Course management.md#course-settings)
* [Enrolling Students](Course management.md#enrolling-students)
    * [Using the CSE IT Classlists Tool](Course management.md#using-the-cse-it-classlists-tool)
    * [Uploading a CSV File](Course management.md#uploading-a-csv-file)
* [Enrolling Teaching Assistants](Course management.md#enrolling-teaching-assistants)
* [Additional Course Management Features](Course management.md#additional-course-management-features)

### [Create an Assessment](Create an assessment.md)

* [Install Assessment](Create an assessment.md#install-assessment)
    * [Import from Tarball](Create an assessment.md#import-from-tarball)
    * [Create from Scratch](Create an assessment.md#create-from-scratch)
* [Assessment Page](Create an assessment.md#assessment-page)
* [Edit Assessment](Create an assessment.md#edit-assessment)
    * [Basic](Create an assessment.md#basic)
    * [Handin](Create an assessment.md#handin)
    * [Penalties](Create an assessment.md#penalties)
    * [Problems](Create an assessment.md#problems)
    * [Advanced](Create an assessment.md#advanced)
* [Add Problems](Create an assessment.md#add-problems)
* [Make a Submission](Create an assessment.md#make-a-submission)
* [View, Annotate, and Grade a Submission](Create an assessment.md#view-annotate-and-grade-a-submission)
    * [View a Submission](Create an assessment.md#view-a-submission)
    * [Annotate a Submission](Create an assessment.md#annotate-a-submission)
    * [Assign a Grade by Annotation](Create an assessment.md#assign-a-grade-by-annotation)
    * [Stacking Annotations](Create an assessment.md#stacking-annotations)

### [Create an Autograder](Create an autograder.md)

* [Setup](Create an autograder.md#setup)
    * [Create an Assessment](Create an autograder.md#create-an-assessment)
    * [Set the Handin Filename](Create an autograder.md#set-the-handin-filename)
    * [Add some Problems](Create an autograder.md#add-some-problems)
    * [Add an Autograder](Create an autograder.md#add-an-autograder)
* [A Minimal Autograder](Create an autograder.md#a-minimal-autograder)
    * [grader.py](Create an autograder.md#graderpy)
    * [autograde.tar](Create an autograder.md#autogradetar)
    * [Makefile](Create an autograder.md#makefile)
* [Upload the Autograder to Autolab](Create an autograder.md#upload-the-autograder-to-autolab)
* [Test the Autograder](Create an autograder.md#test-the-autograder)
    * [Upload a Correct Solution](Create an autograder.md#upload-a-correct-solution)
    * [Upload an Incorrect Solution](Create an autograder.md#upload-an-incorrect-solution)
    * [Upload a Problematic Solution](Create an autograder.md#upload-a-problematic-solution)
        * [Crashing the Grader](Create an autograder.md#crashing-the-grader)
        * [Getting the Grader's Source Code](Create an autograder.md#getting-the-graders-source-code)
        * [Setting an Arbitrary Score](Create an autograder.md#setting-an-arbitrary-score)
* [Formatted Feedback](Create an autograder.md#formatted-feedback)
* [Embedded Forms](Create an autograder.md#embedded-forms)
    * [Create the HTML Form](Create an autograder.md#create-the-html-form)
    * [Upload the HTML Form](Create an autograder.md#upload-the-html-form)
    * [Considerations for the Autograder](Create an autograder.md#considerations-for-the-autograder)
    * [Embedded form with file submission (UB Feature)](Create an autograder.md#embedded-form-with-file-submission-ub-feature)

### [Grader Assignment Tool](Grader Assignment Tool.md)

* [Accessing the GAT](Grader Assignment Tool.md#accessing-the-gat)
* [Import Your Course from Autolab](Grader Assignment Tool.md#import-your-course-from-autolab)
* [Manage People](Grader Assignment Tool.md#manage-people)
* [Manage Conflicts of Interest](Grader Assignment Tool.md#manage-conflicts-of-interest)
* [Create a Grading Assignment](Grader Assignment Tool.md#create-a-grading-assignment)
* [View a Grading Assignment](Grader Assignment Tool.md#view-a-grading-assignment)
* [Archive a Grading Assignment](Grader Assignment Tool.md#archive-a-grading-assignment)

### [UB Course Sections](UB course sections.md)

* [Configure your Course Sections](UB course sections.md#configure-your-course-sections)
    * [Magic Import](UB course sections.md#magic-import)
    * [Create a Section Manually](UB course sections.md#create-a-section-manually)
* [Configure an Assessment](UB course sections.md#configure-an-assessment)
* [How it Looks to Students](UB course sections.md#how-it-looks-to-students)

### [Create an Autograding Image](Create an autograding image.md)

* [Creating your Own Image](Create an autograding image.md#creating-your-own-image)
* [Testing your Image](Create an autograding image.md#testing-your-image)
* [Add your Image to Autolab](Create an autograding image.md#add-your-image-to-autolab)

### [The Autograding Process](The autograding process.md)

* [Sending the Job to Tango](The autograding process.md#sending-the-job-to-tango)
* [Tango Queues the Job](The autograding process.md#tango-queues-the-job)
* [Running the Job](The autograding process.md#running-the-job)
* [Autodriver](The autograding process.md#autodriver)
* [After the Job is Finished](The autograding process.md#after-the-job-is-finished)

## Sample Autograders

Sample autograders are located in the `sample_files` directory of the [GitHub repository](https://github.com/UB-CSE-IT/Autolab-Public-Documentation) for this documentation. These are referenced throughout the documentation. Some
autograder directories contain multiple correct and/or incorrect solutions to demonstrate how the grader works in
different situations.

* [autograder0](https://github.com/UB-CSE-IT/Autolab-Public-Documentation/tree/main/sample_files/autograder0) is the most minimal autograder just to give you an idea of the format.
* [autograder1](https://github.com/UB-CSE-IT/Autolab-Public-Documentation/tree/main/sample_files/autograder1) is a more realistic autograder that grades 3 problems differently, but it's
  not robust enough for real use.
* [autograder2](https://github.com/UB-CSE-IT/Autolab-Public-Documentation/tree/main/sample_files/autograder2) demonstrates how to use the settings.json file to get metadata about the
  submission.
* [autograder3](https://github.com/UB-CSE-IT/Autolab-Public-Documentation/tree/main/sample_files/autograder3) demonstrates how to grade an embedded form submission.
* [autograder4](https://github.com/UB-CSE-IT/Autolab-Public-Documentation/tree/main/sample_files/autograder4) demonstrates how to create an embedded form that allows submitting a file.

## Private documentation

For IT staff, there's also a private internal documentation repository available
here: <https://github.com/UB-CSE-IT/Autolab-Internal-Docs>.