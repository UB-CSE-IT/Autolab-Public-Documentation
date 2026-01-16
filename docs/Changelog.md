This document logs the changes made in each UB Autolab version. You can view the detailed commit history in
the [GitHub repository](https://github.com/UB-CSE-IT/Autolab).

## 2026.0.0

Deployed 2026-01-16

- New feature: A per-assessment submission cooldown can be configured to discourage rapid low-effort submissions
- New feature: All annotations for an assessment can be exported to a CSV file from the "Manage Submissions 2.0" page
- UX: When creating extensions, the user matching the email address in the user selection field will be selected by
  default when the field loses focus. Multiple comma-separated email addresses can be pasted, and each user will be
  added to the selection.
- UI: Reduce the header size of the "Manage Submissions 2.0" page
- Bug fix: Don't show duplicate directories in the submission preview file tree when a submission contains a directory
  named "."
- Bug fix: Allow GitHub submissions from repositories and branches with underscores in their names

## 2025.1.2

Deployed 2025-11-25

- Bug fix: Directories containing macOS metadata files will be displayed when previewing a submission
- UX: Directories are listed before files when previewing a submission
- UI: The default page selector is only shown when previewing PDF submissions

## 2025.1.1

Deployed 2025-10-13

- New feature: A default page number and problem name can be specified when annotating submissions to make grading
  faster
- New feature: Annotated PDF submissions can be downloaded in bulk
- UX: Files are sorted alphabetically when previewing a submission
- UI: Added a link to this changelog in the footer
- Bug fix: Submitting a corrupt archive no longer prevents previewing the contents of later submissions

## 2025.1.0

Deployed 2025-08-30

- New feature: Assessment dependencies: In Edit Assessment > Advanced, an assessment can require achieving a minimum
  score on another assessment before students may submit it.

## 2025.0.5

Deployed 2025-06-17

- Bug fix: Assessment dates are validated to be compatible with MySQL TIMESTAMP type to prevent soft-locking the course.
- Bug fix: Code difference viewer now supports non-ASCII files.
- UX: Handin form is reset when navigating with browser back button.
- Literature: Documentation links throughout Autolab have been updated to refer to this interactive documentation site.
- New feature: Add support for Matomo Analytics.

## 2025.0.4

- Performance improvement: Assessment page loads much faster (~700 ms to ~100 ms).
- Performance improvement: Manage course page loads much faster.
- Literature: Update VM images hint to refer to our
  new [Autograding Images repository](https://github.com/UB-CSE-IT/Autograding-Images)

## 2025.0.3

- UX: Lines of code longer than 80 characters won't be highlighted by default. (Some users were confused about what
  this meant.)
- Literature: Footer includes a link to our new [status page](https://status.cse.buffalo.edu/status/autolab), and the
  contact page has more information about source code.

## 2025.0.2

- Bug fix: Previewing GitHub submissions has been fixed.
- UI: Updated favicon on error pages to be blue instead of red.

## 2025.0.1

Deployed 2025-01-27

- Literature: Display warning about not sharing MOSS results with students.
- New feature: Import assessment groups from CSV file.
- UX: Manage Submissions 2.0 search is now case-insensitive and suggests matching users while searching.
- UI: Fixed orange writeup badge and red attachment section header to be blue.
- Convenience: Course/assessment logs are no longer rotated (instructors want all logs in one file).
- Bug fix: GitHub submission URL has been fixed.

## 2025.0.0

Deployed 2025-01-13

This update merges upstream updates from CMU version `3.0.0`. Notable changes include:

- A difference viewer has been added to highlight changes between submissions.
- Students can export all of their submissions globally for archival.
- Students can optionally self-enroll in courses with a join code.
- Courses can be exported.
- Breadcrumbs and page titles have been improved.

## 2024.1.3

- Bug fix: "Dropped course" message will not display on the next page loaded after viewing a dropped course.
- Security improvement: Prevent HTML injection on "grade submissions"
  page. ([Advisory](https://github.com/autolab/Autolab/security/advisories/GHSA-8qhp-jhhw-45r2))
- Security improvement: Prevent instructors from updating grades in an arbitrary
  course. ([Advisory](https://github.com/autolab/Autolab/security/advisories/GHSA-rjg4-cf66-x6gr))

## 2024.1.2

- New feature: GitHub submission metadata will be included in `settings.json`.

## 2024.1.1

- UI improvement: Maximum total score is rounded to 3 decimal places in submission history table.
- Bug fix: Autograder feedback will be updated upon regrading a submission even if the autograder has an error.
  preventing grading. (Useful for instructors to debug their autograders.)

## 2024.1.0

- Bug fix: Course and assessment attachments will be ordered explicitly.
- Security improvement: The session cookie explicitly sets the `secure` and `same_site` attributes.
- Bug fix: Fixed maximized code preview bug where the customizable layout wouldn't load properly after refreshing the
  page if you maximized the code panel.
- Literature: Removed legacy Autolab message.
- Literature: Mention that CSE IT doesn't control course enrollment on contact page.
- Performance improvement: Manage Submissions 2.0 filters to latest submission by default to avoid loading more
  submissions than are usually necessary.

## 2024.0.9

Deployed 2024-03-16

- Bug fix: The "Group options" link under "Options" on an assessment has been corrected.

## 2024.0.8

Deployed 2024-03-09

- Bug fix: Submissions can now be sorted by individual problem scores on the "Grade Submissions" page if there are
  students who haven't made a submission.

## 2024.0.7

Deployed 2024-03-02

- Logging improvement: When a student makes a submission, it will be logged to the assessment log. This is in addition
  to the existing autograded score logging.

## 2024.0.6

Deployed 2024-02-24

- Bug fix: When submission archives are extracted to send to MOSS, add a prefix to all file names to avoid dotfiles not
  being included in the glob.

## 2024.0.5

Deployed 2024-02-17

- Convenience improvement: Unique assessment names (used for URLs) are generated more intelligently.
    - E.g., "Lab 1: Intro to Git (practice)" will become "Lab-1-Intro-to-Git-practice" instead of "Lab."
- Logging improvement: When a student downloads an attachment for an assessment, it will be logged to the assessment
  log.
- Bug fix: Existing users' first and last names will not be updated from roster uploads.

## 2024.0.4

Deployed 2024-02-10

- New feature: Scoreboards can optionally include instructors.
- Bug fix: Attempting to RegEx match an integer for pluralization (ApplicationController#pluralize) will no longer raise
  an exception.
- Literature: Messages for when *downloading attachments is restricted to section times AND handins are disabled for an
  assessment* have been clarified for both students and instructors.
- Literature: The prior courses message has been updated to reflect our plans to shut down the previous version:
  "Courses prior to fall 2023 are still available on our legacy site at autograder.cse.buffalo.edu. You must be on a UB
  network to access it. The legacy site will shut down during the summer of 2024, so please download any work you'd like
  to keep before then."

## 2024.0.3

Deployed 2024-01-18

- Security improvement: Instructors cannot upload arbitrary Ruby scripts for courses or assessments anymore. This
  feature is still available if you contact CSE IT, but it poses too much of a security risk if left unsupervised.
- Literature: The wording on 404 and 500 error pages has been updated, and debugging information is shown to admins
  only.

## 2024.0.2

Deployed 2024-01-16

- New feature: The "Manage Submissions 2.0" page can now filter to latest submissions only.
- Performance improvement: The "Edit Assessment" page loads much quicker because we are no longer scanning each
  submission to see if there are any annotations.
- Security improvement: Course email features have been disabled.
- Bug fix: The OAuth app authorization page title is set correctly instead of "Authorize @pre_auth.client.name".
- Literature: Authorized application permissions have been explained better.

## 2024.0.1

_This version was never officially deployed into production._

- New feature: Introduce UB versioning separate from CMU versioning. The UB and CMU versions are included in the footer
  of nearly every page.

## Prior to UB Versioning

Before implementing UB versioning separate from CMU versioning, there were many improvements made to UB's variant of
Autolab.

### January 2024

- CMU Merge: Updates made upstream by CMU from May 23, 2023 through January 8th, 2024 have been merged into UB
  Autolab.
- Security/performance: Schedulers were disabled to improve both security and performance. Nobody ever used this
  feature, but it posed an arbitrary code execution risk.
- Bug fix: For group submissions, the submitter is the owner of the Tango job, rather than the first member of the
  group.
- Literature: The capitalization of "Autolab Self-Service Portal" was standardized.
- Literature: The capitalization of "Autolab" was standardized. (There was only one location where it was incorrect.)
- Performance: Heap analytics has been removed. We never used this, and it was only causing silent client-side errors.

### December 2023

- New feature: The "Manage Submissions 2.0" page has been implemented with much faster performance than the previous
  version (~78x faster).
- Convenience improvement: After a bulk grade import, instructors are offered to release all grades immediately.
- Bug fix: Update the frontend to allow annotations without a problem specified (for general comments without scores).
- Bug fix: IP addresses from UB VPN users will be logged correctly.

### October 2023

- Bug fix: Semantic feedback won't be parsed as JSON unless it's a Ruby hash (JSON object).
- Convenience improvement: If a user was created automatically without having their first name specified, their
  first name will automatically be updated the first time they log in.
- UI improvement: The maximum total score is included in the submissions table header.
- UI improvement: The "since" datestamp formatting has been improved on the Tango jobs page.
- Bug fix: Instructors can view detailed information about Tango jobs for their own courses.
- Bug fix: Submissions can always be downloaded, even if handins are disabled.
- UI improvement: A detailed error message is shown if a student submits when they aren't allowed to.
    - Perhaps they loaded the page before the due date, but clicked submit after the due date.

### September 2023

- New feature: UB Course Sections have been implemented to granularly specify assessment start and end times for each
  section.
- New feature: Attachments can be restricted to only be available during a student's lecture/section time.
- New feature: Per-assessment section offsets can be specified.
- Literature: Documentation for the UB Course Sections feature has been linked in the relevant settings panel.
- Bug fix: The author of an annotation will not be updated if the annotation is moved.
- Bug fix: Annotation authors cannot be spoofed.
- Security improvement: The handin directory and handin filename are validated more strictly.
- Security improvement: The writeup and handout cannot be set to a file.
  Resolves [this path traversal vulnerability](https://github.com/autolab/Autolab/security/advisories/GHSA-h8wq-ghfq-5hfx).
- Bug fix: Tango will receive a unique settings.json filename to avoid collisions between students.
- UI improvement: GitHub submissions will be the default handin tab, if available.
- UI improvement: 100 GitHub repos (API maximum) will be queried from GitHub instead of 30 to make it easier to find
  one in the dropdown list.
- API update: Course section management (non-public) endpoints have been added.

### August 2023

- New feature: The Grader Assignment Tool (part of the Autolab Self-Service Portal) API endpoints have been created.
- Convenience improvement: Multiple problems can be added to an assessment rapidly without returning to the edit page.
- Bug fix: First and last names have a maximum length enforced.
- Convenience improvement: Instructors are warned when an assessment has an autograder but no problems.
- UI improvement: An "extra credit" warning displays when manually grading a problem with a higher score than the
  maximum. This doesn't prevent saving because some professors want to assign extra credit.

### July 2023

- Bug fix: Rapid submissions won't result in duplicate version numbers.
- Literature: Links to general documentation have been added to the contact/resources page.

### May and June 2023

We began working on the new version of UB Autolab in May 2023. The previous version was over 5 years old and very
outdated. CMU added many new features and fixed many security issues since our original deployment.

This major renovation included:

- Restyling the entire application to fit UB's blue color scheme
- Updating wording and links throughout
- Implementing Shibboleth authentication
- Improving logging
- Adding settings.json to every submission
- Disabling self-registration
- Configuring the Docker environment (spanning 7 different servers!)
