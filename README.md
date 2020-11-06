This guide was originally written for a UIUC CS hosted GitHub Enterprise and the repository
hosted on that site. It has been moved to https://github.com/uillinois-community/gitForCourses

# Git for Courses

Documents in this repository describe how different Illinois courses have used
git for student assignment distribution, collection, grading, regrades, etc.

For feedback, contact Dave Mussulman (<mussulma@illinois.edu>). Issues and PR's welcomed.

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Git for Courses](#git-for-courses)
	- [Services](#services)
		- [GitHub.com](#githubcom)
	- [GitLab](#gitlab)
	- [GitHub Enterprise](#github-enterprise)
	- [Terminology](#terminology)
	- [Assignments and repositories](#assignments-and-repositories)
		- [One organization per semester, one repository per student, one directory per assignment](#one-organization-per-semester-one-repository-per-student-one-directory-per-assignment)
		- [One organization per semester, one repository per student-assignment](#one-organization-per-semester-one-repository-per-student-assignment)
		- [One organization per semester, one repository per student, one branch per assignment](#one-organization-per-semester-one-repository-per-student-one-branch-per-assignment)
	- [Who creates the environment for the assignment?](#who-creates-the-environment-for-the-assignment)
		- [Student provisions, student triggered](#student-provisions-student-triggered)
		- [Course provisions, course triggered](#course-provisions-course-triggered)
		- [Course provisions, student triggered](#course-provisions-student-triggered)
	- [Assignment distribution](#assignment-distribution)
		- [Course triggers distribution](#course-triggers-distribution)
		- [Student triggers distribution](#student-triggers-distribution)
		- [Provisioning triggers distribution](#provisioning-triggers-distribution)
	- [Assignment collection and grading](#assignment-collection-and-grading)
		- [Course triggers collection and grading](#course-triggers-collection-and-grading)
		- [Student triggers collection and grading](#student-triggers-collection-and-grading)
	- [How are grades returned?](#how-are-grades-returned)
		- [Committing the feedback](#committing-the-feedback)
		- [Handling inside of git but not a commit](#handling-inside-of-git-but-not-a-commit)
		- [Handling outside of a git server](#handling-outside-of-a-git-server)
		- [Regrade requests?](#regrade-requests)
	- [Authorization, Permissions and AD groups](#authorization-permissions-and-ad-groups)
	- [Gotcha's we have learned](#gotchas-we-have-learned)
	- [Dave opinion](#dave-opinion)
	- [Other resources](#other-resources)

<!-- /TOC -->

## Services

Git is a version control systems for tracking changes in computer files and coordinating work on these files among multiple people. [Wikipedia](https://en.wikipedia.org/wiki/Git)

Git is often paired with a server component that gives a web front-end to the data, handles collaboration and discovery across projects, and adds more collaborative features like issues, wikis, web-hosting, etc.

Some of these used at Illinois:

### GitHub.com

The venerable cloud-based git service, [github.com](github.com), has a [GitHub Education](https://education.github.com) program which supplies free hosting for students and courses. The GitHub username is independent from your Illinois netid or password.

It's not entirely clear the FERPA risks of using GitHub's cloud service for student sensitive data.

## GitLab

Engineering IT hosts a GitLab Community Edition service at https://gitlab.engr.illinois.edu Login uses the Illinois NetID and password.

Because the service is running on-premise by Illinois staff, there are no third-party FERPA risks for using gitlab.engr with courses.

## GitHub Enterprise

Computer Science has been piloting courses using an on-campus GitHub Enterprise service at https://github-dev.cs.illinois.edu. Login uses the Illinois NetID and password. This service can leverage Active Directory groups for permissions, such as roster groups or course staff groups (managed at https://my.engr.illinois.edu/classtools).

Because the service is running on-premise by Illinois staff, there are no third-party FERPA risks for using github-dev.cs with courses.


## Terminology

For help muddling through what git and the git servers call their things, see the [terminology](terminology.md) page. This guide will largely follow the GitHub terminology.


## Assignments and repositories

You have options on how you create assignments around git repositories.

### One organization per semester, one repository per student, one directory per assignment

- The organization is named per semester, i.e. `cs225-fa18`.
- Repositories in the org are named by `netid` of the student.
- Assignments are handled as directories inside the repo.
    - Example: _cs225-fa18:mussulma/lab-intro_

This model is closest to how courses used EWS's subversion service.

Pros:
- 1 repository simplifies the places for the student to look or setup.
- Repositories can inherit organization-wide permissions, such as allowing course staff access to student repos with setup in one place.
- Similar setup as subversion.ews.illinois.edu for student interaction and course tools.

Cons:
- Repositories need to be created by organization owners (which is also a positive, see below).
- Any activities that hook off of changes (grading, CI) need to inspect at the directory level, not at the base repo.
- Students may work on different directories in different locations (lab assignments on EWS computer, MP assignments on their laptop) requiring them to sync up their various copies so stale work doesn't get submitted.

Who's done this:
- [CS225](cs225/checklist.md)

### One organization per semester, one repository per student-assignment

- The organization is named per semester and assignment, i.e. `cs225-fa18-mp1`
- Repositories are named by assignment and `netid` of the student.
    - Example: _cs125-fa18:MP2-mussulma_
- Assignments use the whole repo.

This is the model that [GitHub Classroom](https://classroom.github.com) uses. It could also be extended to one organization per assignment if further isolation was desirable.

Note: Some IDEs better support one project per repository rather than shared projects in a single repository.

Pros:
- Repos are atomic to the whole assignment, so multiple repos checked out in different places for different assignments is less of a problem.
- Grading and commit hooks are more straightforward.

Cons:
- Students need to `git clone` for each repository. They could get confused on what is where.
- For large classes with many assignments, this could mean a large list of repositories.

Who's done this:
- [CS125](scenarios/cs125.md)

### One organization per semester, one repository per student, one branch per assignment

This twist on the __one repo per student__ model uses branches instead of just directories to isolate assignments. Branches are a more git-like way to isolate different things in the same repo.

Pros:
- Branches in different locations are a cleaner split (git sync-wise) than directories.
- Browsing branches is easily supported in the web user interface.
- Commit hooks and CI might be configured easier per-branch than per-directory.

Cons:
- Switches between branches is not as intuitive as changing directories.
- Branch switching can remove files from the local view, or require more diligent committing before branching than a novice git user could support.
- It's not really what branches are designed for.

Who's done this:
- [Harvard CS50](scenarios/harvard-cs50.md)

## Who creates the environment for the assignment?

Whether it's repos or organizations or branches, someone is going to need to do the provisioning for the assignment for the student. That usually involves:
- creating the place for the assignment
- setting permissions for that place (authorized people in, unauthorized people out)
- tracking where that place is to interact with it later
- other setup metadata (settings, initial files, etc.)

There are three identified models for this:

### Student provisions, student triggered

In this model, the student creates a repository and does the configuration for it (typically adding in the course staff). This could live in a student-owned organization, or in a course-owned organization where students have create repo permissions.

Pros:
- Student initiation means course staff are not involved in bootstrapping git for the students.
- Student (presumably) knows what they made and where they created it (as opposed to figuring out their part in a bigger course collection.)

Cons:
- Course staff cannot enforce naming or permissions on the repository. Extra work might be necessary to map `netid` to repositories.
- Tracking where these repos live can be more complicated than just getting a list of all the repositories in an organization.
- Student must configure the permissions properly to let course access their repo.
- No governance over this repository after the semester ends.
- Locations, naming, permissions, etc. can be very inconsistent.

Who's done this:
- Darko Marinov with CS428/429

This seems to work best for small classes (where tracking the number of repos to grade is easier) and for groups (where they decide and control the membership). This implies some knowledge of git configuration, which might be real-life-applicable for software engineering courses but complex for VCS newbies.

### Course provisions, course triggered

In this model, the course owns and creates all of the student repositories. Presumably, a script is run to iterate over a roster and do the same provisioning steps for each student who needs a repository created.

Pros:
- Very consistent naming and permission, with a high degree of control on where and how this is set.
- Ownership of the repo lives with the course. Students can be given access to their repo but not full ownership.

Cons:
- The course staff are required to do something prior to a student using their repository.
    - If that triggers notifications to the student, those might come at unexpected times.
- It assumes the list of students iterating over is known (i.e. a roster). Because of delays in registrations, wait lists, auditors, etc. the roster may not be accurate (especially early in the semester). Also, for dropped students, resources are still created and need to be stewarded.
- Requires extra software to provision the repository.

Who's done this:
- Darko Marinov, Mattox Beckman

### Course provisions, student triggered

This hybrid attempts to marry the consistency of course-owned and provisioned repositories with the flexibility and control that student-triggered provisioning entails.

This is the model Google Classroom uses. The instructor sets up the overall template for the assignment and creates a link the students can click-into to self-provision.

Pros:
- Incorporates advantages of both previous models while minimizing their disadvantages.

Cons:
- Requires another service to accept the student request and run something on the course's behalf. This is not part of the vanilla git servers.
- Because it's not tied to a roster, anyone could sign up to provision a space in the course even if they aren't enrolled.
    - Further validation may be necessary.

Who's done this:
- CS125 (Google Classroom)
- [CS225 (open-sourced web micro-service running on edu.cs.illinois.edu)](cs225/checklist.md)

## Assignment distribution

All of these methods assume provisioning has already been done, although distribution may be included in the initial provision (or done immediately afterwards, for example with an introduction to git lab).

Often coding assignments will start with some "seed" code that sets up the problem, or the environment. It may give stub code to fill in, or tests to run locally against it.

Distribution implies the method in which the student gets that seed code into their repository to start working on it. Another workflow involves course staff updating that seed code (perhaps to fix bugs after the assignment was released) and the method where students update their copies.

### Course triggers distribution

Similar to course triggered provisioning, course triggered distribution implies the course has a list of locations to copy the seed files into. The course already has a checkout of each student repository locally. This method of distribution involves the course puting seed files directly into the student repository, `git add`, `git commit`, and `git push` for each.

Since this commit would change the repository independent from the checked out version, eventually a `git merge` would be required (likely as a `git pull`). Even though the course has triggered the distribution, the student would still need to run some commands to update their repo.

However, because the distribution usually only adds files, the risk of merge conflicts is low.

### Student triggers distribution

Since a merge is required for new files to be added to the repository anyway, a more git-friendly method involves the course making the seed code available on another repository.

The student is responsible for fetching and merging from the distribution repository (or branch) into their location.

This method is useful because it can be repeated if the distribution source repository is updated. Then, the typical git merge resolution methods apply so there's less chance of conflict.

**Example:**

In the same course organization, assignments are released in the `_release` repository (public to members of the organization). CS225 created new assignments as branches in this repo (i.e. `_release/lab3`.)

The student has a private `netid` repository in the same organization. As a one time setup, the student adds the release repo as a remote:

`git remote add release https://github-dev.cs.illinois.edu/cs225sp18/_release.git`

Then to distribute (or update) the released assignment, the student runs:

> `git fetch release`
>
> `git merge release/lab3 -m "Merging initial lab3 files"`


### Provisioning triggers distribution

For repository-per-assignment models, the act of provisioning can also trigger "seeding" the directory with the distribution files.

GitHub Classroom does this well -- but only on github.com. Classroom, when used with GitHub Enterprise, does not support this file seeding. (Classroom uses a Source Import API that is in "public preview" on github.com but is not in the GHE product.)

Cons:
- Provisioning is a one-time step. If updates to the distribution files needs to happen, another method maybe preferred.

## Assignment collection and grading

### Course triggers collection and grading

There are a few strategies for course-triggered collection of the assignment:

#### Pull at due date

This method involves the instructor doing a pull for the student's repositories at the time of collection and working off those local pulls for grading.

Pros:

* This technique is beneficial because it doesn't care about the commit timestamps supplied by the student, just the overall state of the repository when the assignment is collected.
* Simple, instructor action triggered (it's just a pull).
* Grading logic can be based off of HEAD (the last thing committed).

Cons:

* Impossible to re-run the collection later.
* Actual collected-time may vary depending on when triggered or when downloaded (based on Internet speed or volume of data to pull) which may affect fairness and accuracy.
* Relies on what is on the server, which wouldn't reflect a student's commit that they had made locally but had not yet pushed.
* Since this pull is specific to a due date for an assignment, you may wish to leave it downloaded and not update that copy. That could lead to duplicated student pulls on your computer and wasted space.

#### Server inquiry based on commits before the due date

Rather than a traditional git pull, use the GitHub API to search for commits before the due date. Because this uses push log data on the GitHub server, it's safer to trust than only looking at timestamps in the git repository itself.

Pros:

* Repeatable results
* Can be used to inspect any previous commit in a repository. Does not require downloading and storing specific pulls.
* Searches for commits based on push date rather than git commit timestamps. The push dates are recorded by the server and are not student changeable.

Cons:
* Requires interactions with the GitHub server to find the specific commits.
* Uses the GitHub API rather than (just) traditional git to correspond to push times rather than git commit times.
* GitHub and GitHub Enterprise specific git server implementation, although other git servers may also have APIs to this data.

The [GitHub repos / listCommits API call](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#list-commits) has an "until" argument that we found references the commit time on the server's database (which corresponds to the "push" time of the user). Logic can be created to scan the list of commits (backwards) to find the latest commit with the file(s) to be graded. [Example relevant code from CS 225's Zephyr autograder.](https://github-dev.cs.illinois.edu/cs225-staff/zephyr/blob/master/zephyr-code-runner/fetchStudentFiles-git-v2.js#L103)

#### Inspect the student repositories for commit dates (risky)

This method uses the `git log` to identify commits before the due date. Because git commit times are based on computer timestamps, which could be altered by the student, this is the least trusted method. If you're not using one of the above methods for collecting the correct commits, students could alter their repositories to post-date commit dates, submit work after the fact (but before grading), etc.

Cons:
* Searches for "commits before the due date" in a method that might be manipulated by student.
* Least preferred method of collection.

### Student triggers collection and grading

* Continuous Integration grading?
* Triggers by non-git/GitHub tools to do just-in-time pulls and grading

## How are grades returned?

If you return grades and feedback inside the student repository, you'll need to add/commit/push and hope there aren't merge conflicts to resolve.

If grades are returned out of band outside of GHE, it means the grading doesn't need to change the student repository. But that means student would need to check another location for their grades.

### Committing the feedback

If you update the student repository with the grading feedback, the student will need to know how to resolve merge conflicts. (Basically, training them how to do a pull before a push.)

One way to make that easier conflict-wise, is to push to a different branch (or different repository) than normal. This obscures finding the feedback a little, but it's still easy to find (especially when reading them via the web interface).

CS225 used a different repository for grade delivery for Spring 2018 (which fed into other web-based views for the student gradebook). For Fall 2018, they will use a branch in the main student repository.

### Handling inside of git but not a commit

Issues could be useful here.

If continuous integration is used for grading, a grade report could be included in the CI workflow.

### Handling outside of a git server

Many courses use an LMS gradebook, or other web resources for tracking student progress.

One simple problem is to give the feedback and grade for the assignment in other methods than


### Regrade requests?

## Authorization, Permissions and AD groups

## Gotcha's we have learned

- Users don't exist until they first login
- Adding someone to an organization or repository sends them an email. That might be unexpected.
    - Early provisioning is smart from a course perspective, but might trigger the students to respond or think they need to respond before the course is ready.
- force pushing, i.e. rewriting git history, should be disabled for student repositories. This is especially true if grading is done based on timestamps from the git log.
- GitHub Enterprise supports disabling 'force pushes' for repositories or organizations. That is, not letting the user rewrite git history by pushing without a proper merge.
-- At the moment, this is a GitHub Enterprise admin configuration and isn't toggeable by the organization or repo admin.


## Dave opinion

- Student triggered activities are better than course triggered ones. It solves two problems:
    - Courses not knowing exactly who their students are (at the needed time) or requiring things to be done (and redone and redone) in the proper sequence for the tools to be useful.
    - Students unsure the state of their assignment (is it released? was it graded?) Those tasks are very deterministic (yes it worked, no it failed because X) and supported because they can be repeated with course staff.
- The passive "leave it there and we'll grade it" is less effective than student triggered grading because of the immediacy of feedback. That's especially true if something went wrong (student mistake or uploaded wrong code) so that it could be corrected and regraded.

## Other resources

- Matt West's [Git Quick Reference](http://lagrange.mechse.illinois.edu/git_quick_ref/)
