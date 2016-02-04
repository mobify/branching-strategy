# Release Deployment

Use this strategy for projects where features get bundled into a release and then
deployed all at once.

We follow the [**Gitflow**](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
workflow as closely as possible. This page showcases common development scenarios
and how to deal with them from a branching point of view.

- [Branches Overview](#branches-overview)
- [Use Cases](#use-cases)
  - [Develop a new feature](#develop-a-new-feature)
  - [Develop multiple features in parallel](#develop-multiple-features-in-parallel)
  - [Create and deploy a release](#create-and-deploy-a-release)
  - [Change in plan, pull a feature from a release](#change-in-plan-pull-a-feature-from-a-release)
  - [Change request](#change-request)
  - [Production hot fix](#production-hot-fix)
  - [Production hot fix](#production-hot-fix)
- [Migrate a legacy project](#migrate-a-legacy-project)

## Branches Overview

**TBD: Insert diagram**

| Branch        | Protected? | Commits Allowed? | Base Branch | Description    |
| :-------------|:-----------|:-----------------|:------------|:---------------|
| `master`      | YES        | NO               | N/A         | What is live in production (**stable**).<br/>A pull request is required to merge code into `master`. |
| `develop`     | YES        | NO               | `master`    | The latest state of development (**unstable**). |
| feature       | NO         | YES              | `develop`   | Cutting-edge features (**unstable**). These branches are used for any maintenance features / active development. |
| `release-vX.Y.Z` | NO      | NO               | `master`    | A temporary release branch that follows the [semver](http://semver.org/) versioning. This is what is sent to UAT.<br/>A pull request is required to merge code into any `release-vX.Y.Z` branch. |
| bugfix        | NO         | YES              | `release-vX.Y.Z` | Any fixes against a release branch should be made in a bug-fix branch. The bug-fix branch should be merged into the release branch and also into develop. This is one area where we’re deviating from GitFlow. |
| `hotfix-*`    | NO         | YES              | `master`    | These are bug fixes against production.<br/>This is used because develop might have moved on from the last published state.<br/>Remember to merge this back into develop and any release branches. |

## Use Cases

### Develop a new feature

**TBD: Insert diagram**

1. Make sure your `develop` branch is up-to-date

   ```
   $ git checkout develop
   $ git fetch
   $ git merge origin/develop
   ```

1. Create a feature branch based off of `develop`

   ```
   $ git checkout -b MYTEAM-123-new-documentation
   $ git push --set-upstream MYTEAM-123-new-documentation
   ```

1. Develop the code for the new feature and commit. Push your changes often. This
   allows others to see your changes and suggest improvements/changes as well as
   provides a safety net should your hard drive crash.

    ```
    $ ... make changes
    $ git add -A .
    $ git commit -m "Add new documentation files"
    $ ... make more changes
    $ git add -A .
    $ git commit -m "Fix some spelling errors"
    $ git push
    ```

1. Navigate to the project on [Github](www.github.com) and open a pull request with
   the following branch settings:
   * Base: `develop`
   * Compare: `MYTEAM-123-new-documentation`

1. When the pull request was reviewed, merge and close it and delete the
   `MYTEAM-123-new-documentation` branch.

### Develop multiple features in parallel

There's nothing special about that. Each developer follows the above [Develop a new feature](#develop-a-new-feature) process.

### Create and deploy a release

**TBD: Insert diagram**

1. Make sure your `master` and `develop` branches are up-to-date

   ```
   $ git checkout master; git pull
   $ git checkout develop; git pull
   ```

1. Merge `master` into `develop` to ensure the new release will contain the latest production code

   ```
   $ git checkout develop
   $ git merge master
   ```

1. Create a new `release-vX.Y.Z` release branch off of `develop`

   ```
   $ git checkout -b release-vX.Y.Z
   $ git push --set-upstream release-vX.Y.Z
   ```

1. Stabilize the release by using bugfix branches off of the `release-vX.Y.Z` branch
   (the same way you would do a feature branch off of `develop`).

   ```
   $ git checkout release-vX.Y.Z
   $ git checkout -b fix-label-alignment
   $ git push --set-upstream fix-label-alignment
   ... do work
   $ git commit -m "Adjust label to align with button"
   $ git push
   ```

1. When the code is ready to release, navigate to the project page on Github and
   draft a new release with the following settings:
   * Tag version: `vX.Y.Z`
   * Target: `release-vX.Y.Z`
   * Release title: `Release vX.Y.Z`
   * Description: Include a high-level list of things changed in this release.

   Click `Publish release`.

1. Make sure `master` is up to date.

    ```
    $ git checkout master
    $ git fetch
    $ git merge origin/master
    ```

1. Merge the `release-vX.Y.Z` branch into `master`.

    ```
    $ git merge release-vX.Y.Z
    $ git push
    ```

1. Make sure `develop` is up to date.

    ```
    $ git checkout develop
    $ git fetch
    $ git merge origin/develop
    ```

1. Merge `master` into `develop`.

    ```
    $ git merge master
    $ git push
    ```

**TBD: Discuss**
Mike N: Long-lived release branches, yes/no?

1. Create the release on Github from `master`.

### Change in plan, pull a feature from a release

**TBD: Discuss**
Mike N: That probably means recreating the release branch, unless we have short-lived release branches

### Change request

**TBD: Discuss**
Mike N: That probably means recreating the release branch, unless we have short-lived release branches

### Production hot fix

**TBD: Insert diagram**

1. Make sure your `master` branch is up-to-date

   ```
   $ git checkout master
   $ git fetch
   $ git merge origin/master
   ```

1. Create a hot fix branch based off of `master`

   ```
   $ git checkout -b hotfix-documentation-broken-links
   $ git push --set-upstream hotfix-documentation-broken-links
   ```

1. Add a test case to validate the bug, fix the bug, and commit

   ```
   $ git add -A .
   $ git commit -m "Fix broken links"
   $ git push
   ```

1. Navigate to the project on [Github](www.github.com) and open a pull request
   with the following branch settings:
   * Base: `master`
   * Compare: `hotfix-documentation-broken-links`

1. When the pull request has been reviewed and ![+1'd](images/plus1.png)
   , merge and close it and then delete the `hotfix-documentation-broken-links`
   branch. This can all be done from the Github pull-request page.

**Note: At this point releasing a hotfix is _exactly_ the same as a regular release.
        Use the [Create and deploy a release](#Create and deploy a release) workflow
        replacing `develop` with your hotfix branch `hotfix-documentation-broken-links`.**

## Migrate a legacy project

To migrate any git project to our branching strategy, please follow the instructions
in the [Migration](migration.md) document.
