# Create an Assessment

An Autolab assessment is a graded assignment that students can hand in. They can be used for homework, labs, exams, and
more. They can be manually graded or automatically graded.

## Install Assessment

Navigate to your course dashboard and click `Install Assessment`.

![Dashboard install assessment](screenshots/dashboard_install_assessment.png)

You'll see the various ways you can add an assessment to your course.

![Install assessment options](screenshots/install_assessment_options.png)

### Import from tarball

If you have an assessment from a previous semester, you can export it from the previous course as a tarball and then
import it here. Just click `Browse` and select the tarball.

> ### Side note about "import from file system"
> This feature is only accessible to users who have direct access to the Autolab server, which essentially means we
> don't use it. If there's a particular need for this, contact CSE IT.

### Create from scratch

This is how assessments should be initially created. Click the "Assessment Builder" link to begin.

1. Choose the assessment name, which is what students will see. Naming it something more specific than "Homework 1" is
   recommended. Something like "Homework 1: HTTP Basics" may be more identifiable to students.
2. Choose the assessment category. If you already have categories defined, you can choose one of those from the
   dropdown. Otherwise, you can create a new category by typing in a new name. Categories are used to group assessments.
   Popular categories include Homework, Labs, Exams, and Projects. Separating assessments into categories is useful for
   weighted grading if you want to use some advanced features of Autolab. Otherwise, it's just good for organization.
3. Click `Create Assessment`.

You can change any of these settings later, except for the URL, which will be automatically generated based on the
display name you choose.

![Assessment builder](screenshots/assessment_builder.png)

## Assessment Page

After creating an assessment, you'll be brought to the assessment page. You have a lot of options here. Here's an
overview.

![Assessment page](screenshots/assessment_page.png)

## Edit Assessment

Let's start by looking at everything you can customize about the assessment. Click `Edit Assessment` at the top of the "
Admin Options."

### Basic

The "basic" tab allows you to edit the values you chose while creating the assessment and change some other display
options. They're fairly self-explanatory with the help text below each option. The ability to upload a custom Ruby
file for the assessment is advanced, so you can ignore that for now.

![Assessment basic settings](screenshots/assessment_basic_settings.png)

### Handin

The "handin" tab allows you to configure the start, due, end, and grading deadline dates for the assessment. You can
choose if students are allowed to submit directly from a GitHub repo, change the handin filename, set the maximum
submission size, and disable handins. Each of these is explained in the help text below the option.

![Assessment handin settings](screenshots/assessment_handin_settings.png)

> ### The `Handin filename` field is important
> * It doesn't matter what the file is named on a student's computer; it will be renamed to the handin filename when
    they upload it.
> * If you're using an automatic grader, this is the name of the file that will be accessible in the grading VM.
> * Students will be warned before uploading a file that doesn't match the handin filename's extension.

### Penalties

The "penalties" tab allows you to configure the maximum submissions limit and reduce scores on late submissions.

![Assessment penalties settings](screenshots/assessment_penalties_settings.png)

### Problems

The "problems" tab allows you to add problems to the assessment, which are the actual "questions" students will answer
and receive points for. We'll add some to our new assessment after this overview.

![Assessment problems settings](screenshots/assessment_problems_settings.png)

### Advanced

The "advanced" tab allows you to configure student groups and use an embedded HTML form for submissions.

The official Autolab [Embedded Forms documentation](https://docs.autolabproject.com/features/embedded-forms/) is a good
reference.

![Assessment advanced settings](screenshots/assessment_advanaced_settings.png)