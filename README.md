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

While all of the git servers feature sets are roughly the same, they vary in their terminology and interfaces. This can be confusing if you switch between servers. Here's a cheat sheet:

- **Repository**: A collection of files and their change history in git. A repository, or repo, is a git data structure that looks a lot like a project directory.
    - GitLab calls this a **project**.

- **Organization**: A collection of repositories that are organized in one place (think like a folder of repos) that may share group-based permissions or notifications.
    - GitLab calls these **groups**.

For more info: https://about.gitlab.com/2017/09/11/comparing-confusing-terms-in-github-bitbucket-and-gitlab/

- **Commit**: A git commit records changes to files in the repository. A commit is an atomic event in the repository's history; adds, removes, changes are stored there.

- **Push** or **Pull**: Because git is a distributed version control system, each location where you manage your files (laptop, lab computer, data on the git server) has its own full record of commits. `git push` and `git pull` events move commit histories between sources, sometimes called remotes.

When you sync what's on GitHub down to your local copy of the repository, you do a `git pull`. After you've changed your files and done a local commit, you do a`git push` to send that back up to GitHub.

> A `pull` is actually just two different git operations: a `fetch` and a `merge`, but to go into that means describing branches which I'm avoiding for now.

Also check out the excellent [Pro Git book](https://git-scm.com/book/en/v2).

## Assignments and repositories



## Other resources

- Matt West's [Git Quick Reference](http://lagrange.mechse.illinois.edu/git_quick_ref/)
