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

### One organization per semester, one repository per student assignment

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

This twist on the __one repo per student__ model uses branches instead of just directories to isolate assignments.

Pros:

Cons:

Who's done this:
- [Harvard CS50](scenarios/harvard-cs50.md)

## Who creates the environment for the assignment?


## Assignment distribution



## Assignment collection and grading



## Other resources

- Matt West's [Git Quick Reference](http://lagrange.mechse.illinois.edu/git_quick_ref/)
