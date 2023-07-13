# Autolab Public Documentation

## Introduction

Autolab is an open-source automated code grading platform developed at Carnegie Mellon University and used by
universities around the world. The UB CSE IT department maintains a customized variant of Autolab for use in our
courses. It's hosted on premises at UB, and you can access it at <https://autolab.cse.buffalo.edu>.

This repository will serve as a guide, primarily for instructors, on how to use Autolab.

## Table of Contents

* [Getting Started](Getting%20started.md)
    * [Your First Login](Getting%20started.md#your-first-login)
    * [Create a Course](Getting%20started.md#create-a-course)
* [Course Management](Course%20management.md)
    * [Viewing Your Courses](Course%20management.md#viewing-your-courses)
    * [Course Page](Course%20management.md#course-page)
    * [Manage Course](Course%20management.md#manage-course)
    * [Course Settings](Course%20management.md#course-settings)
    * [Enrolling Students](Course%20management.md#enrolling-students)
        * [Using the CSE IT Classlists Tool](Course%20management.md#using-the-cse-it-classlists-tool)
        * [Uploading a CSV File](Course%20management.md#uploading-a-csv-file)
    * [Enrolling Teaching Assistants](Course%20management.md#enrolling-teaching-assistants)
    * [Additional Course Management Features](Course%20management.md#additional-course-management-features)
* [Create an Assessment](Create%20an%20assessment.md)
    * [Install Assessment](Create%20an%20assessment.md#install-assessment)
        * [Import from Tarball](Create%20an%20assessment.md#import-from-tarball)
        * [Create from Scratch](Create%20an%20assessment.md#create-from-scratch)
    * [Assessment Page](Create%20an%20assessment.md#assessment-page)
    * [Edit Assessment](Create%20an%20assessment.md#edit-assessment)
        * [Basic](Create%20an%20assessment.md#basic)
        * [Handin](Create%20an%20assessment.md#handin)
        * [Penalties](Create%20an%20assessment.md#penalties)
        * [Problems](Create%20an%20assessment.md#problems)
        * [Advanced](Create%20an%20assessment.md#advanced)
    * [Add Problems](Create%20an%20assessment.md#add-problems)
    * [Make a Submission](Create%20an%20assessment.md#make-a-submission)
    * [View, Annotate, and Grade a Submission](Create%20an%20assessment.md#view-annotate-and-grade-a-submission)
        * [View a Submission](Create%20an%20assessment.md#view-a-submission)
        * [Annotate a Submission](Create%20an%20assessment.md#annotate-a-submission)
        * [Assign a Grade by Annotation](Create%20an%20assessment.md#assign-a-grade-by-annotation)
        * [Stacking Annotations](Create%20an%20assessment.md#stacking-annotations)
* [Create an Autograder](Create%20an%20autograder.md)
    * [Setup](Create%20an%20autograder.md#setup)
        * [Create an Assessment](Create%20an%20autograder.md#create-an-assessment)
        * [Set the Handin Filename](Create%20an%20autograder.md#set-the-handin-filename)
        * [Add some Problems](Create%20an%20autograder.md#add-some-problems)
        * [Add an Autograder](Create%20an%20autograder.md#add-an-autograder)
    * [A Minimal Autograder](Create%20an%20autograder.md#a-minimal-autograder)
        * [grader.py](Create%20an%20autograder.md#graderpy)
        * [autograde.tar](Create%20an%20autograder.md#autogradetar)
        * [Makefile](Create%20an%20autograder.md#makefile)
    * [Upload the Autograder to Autolab](Create%20an%20autograder.md#upload-the-autograder-to-autolab)
    * [Test the Autograder](Create%20an%20autograder.md#test-the-autograder)
        * [Upload a Correct Solution](Create%20an%20autograder.md#upload-a-correct-solution)
        * [Upload an Incorrect Solution](Create%20an%20autograder.md#upload-an-incorrect-solution)
        * [Upload a Problematic Solution](Create%20an%20autograder.md#upload-a-problematic-solution)
            * [Crashing the Grader](Create%20an%20autograder.md#crashing-the-grader)
            * [Getting the Grader's Source Code](Create%20an%20autograder.md#getting-the-graders-source-code)
            * [Setting an Arbitrary Score](Create%20an%20autograder.md#setting-an-arbitrary-score)
    * [Formatted Feedback](Create%20an%20autograder.md#formatted-feedback)
    * [Embedded Forms](Create%20an%20autograder.md#embedded-forms)
      * [Create the HTML Form](Create%20an%20autograder.md#create-the-html-form)
      * [Upload the HTML Form](Create%20an%20autograder.md#upload-the-html-form)
      * [Considerations for the Autograder](Create%20an%20autograder.md#considerations-for-the-autograder)

For IT staff, there's also a private internal documentation repository available
here: <https://github.com/UB-CSE-IT/Autolab-Internal-Docs>.

## Sample Autograders

Sample autograders are located in the `sample_files` directory. These are referenced throughout the documentation. Some
autograder directories contain multiple correct and/or incorrect solutions to demonstrate how the grader works in
different situations.

* [autograder0](sample_files/autograder0) is the most minimal autograder just to give you an idea of the format.
* [autograder1](sample_files/autograder1) is a more realistic autograder that grades 3 problems differently, but it's
  not robust enough for real use.
* [autograder2](sample_files/autograder2) demonstrates how to use the settings.json file to get metadata about the
  submission.
* [autograder3](sample_files/autograder3) demonstrates how to grade an embedded form submission.

## Navigation on GitHub

The individual pages don't have tables of contents. You can use the GitHub Outline feature to navigate between sections
of a document.

![GitHub tables of contents](screenshots/github_table_of_contents.png)