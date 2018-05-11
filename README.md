# Git for Courses

Documents in this repository describe how different Illinois courses have used
git for student assignment distribution, collection, grading, regrades, etc.

For feedback, contact Dave Mussulman (<mussulma@illinois.edu>). Issues and PR's welcomed.

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
- [CS225](scenarios/cs225.md)

### One organization per semester, one repository per student-assignment

- The organization is named per semester and assignment, i.e. `cs225-fa18-mp1`
- Repositories are named by assignment and `netid` of the student.
    - Example: _cs125-fa18:MP2-mussulma_
- Assignments use the whole repo.

This is the model that [GitHub Classroom](https://classroom.github.com) uses. It could also be extended to one organization per assignment if further isolation was desirable.

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

### Student provisions, student triggers it

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
- Darko Marinov

This seems to work best for small classes (where tracking the number of repos to grade is easier) and for groups (where they decide and control the membership). This implies some knowledge of git configuration, which might be real-life-applicable for software engineering courses but complex for VCS newbies.

### Course provisions, course triggers it

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

### Course provisions, student triggers it

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
- CS225 (open-sourced web micro-service running on edu.cs.illinois.edu)

## Assignment distribution

### Course triggers it

### Student triggers it

### Happens on provision




## Assignment collection and grading

### Course triggers it

### Student triggers it


## How are graders returned?


### Regrade requests?

## Gotcha's we have learned

- Users don't exist until they first login
- Adding someone to an organization or repository sends them an email. That might be unexpected.
    - Early provisioning is smart from a course perspective, but might trigger the students to respond or think they need to respond before the course is ready.
- force pushing, i.e. rewriting git history, should be disabled for student repositories. This is especially true if grading is done based on timestamps from the git log.


## Other resources

- Matt West's [Git Quick Reference](http://lagrange.mechse.illinois.edu/git_quick_ref/)
