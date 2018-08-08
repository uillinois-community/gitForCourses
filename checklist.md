# Checklist for CS225 method

A quick list for instructors who want to use the CS225 git methods.

## Things you do once a semester:

### Organization setup

These steps should be done once, usually by the lead instructor to setup the organization for the semester.

1. Create a per-semester organization on GitHub Enterprise.
    * https://github-dev.cs.illinois.edu/organizations/new
    * Name it something like `cs999-sp19`
    * You can skip the "Add members" step.
1. Give access to course staff based on AD groups.
    * Under your organization, click **Teams**.
    * Choose **New team**
    * Create an `Instructors / TAs` team with an LDAP group similar to:<br>
    `cn=cs-999-stf,ou=cs,ou=staff,ou=classes,ou=usersandgroups,ou=engineering,ou=urbana,dc=ad,dc=uillinois,dc=edu`
        * Replace `cs-999` with your course pattern.
        * If the course is in a different department, adjust `ou=cs` appropriately. i.e. `cn=ece-999-stf,ou=ece,...`
1. If you use grader groups, setup a team for them as well.
    * `cn=cs-999-grd,ou=cs,ou=graders,ou=classes,ou=usersandgroups,ou=engineering,ou=urbana,dc=ad,dc=uillinois,dc=edu`
1. Adjust the organization settings.
    * From your organization main page, click **Settings**.
    * Under Member privileges on the left:
        * **Uncheck** "Allow members to create repositories for this organization".
        * Set "Default repository permissions" to **None**.
1. Create a `_release` repository in your organization.
    * Set it **Public**.
1. Request force pushes disabled for that organization.
    * Email mussulma@illinois.edu with your organization name to request disabling force pushes for repositories within that organization.

### makeRepo micro-service setup

We have a web micro-service that creates and permissions student repositories in your organization. Do this once to get setup:

1. Fill out the form at https://forms.illinois.edu/sec/8414298
1. Admins will receive that information and manually configure the micro-service for you. They will email back your URL.

### _release repository setup

Tips:
* Use a different git branch for each assignment, and store the files for that assignment in its own directory. That makes updating and merging easier.
* Do your assignment development in another repository (_staging?) whose access is limited to just course staff. Manually copy the files into the release repo branch when they're ready.