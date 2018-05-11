# Git terminology (with git servers)

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
