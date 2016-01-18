# Release Deployment

Use this strategy for projects where features get bundled into a release and then
deployed all at once.

We follow the [**Gitflow**](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) workflow as closely as possible. This page showcases common development scenarios and how to deal with them from a branching point of view.

- [Branches Overview](#branches-overview)
- [Develop a new feature](#develop-a-new-feature)
- [Develop multiple features in parallel](#develop-multiple-features-in-parallel)
- [Create and deploy a release](#create-and-deploy-a-release)
- [Change in plan, pull a feature from a release](#change-in-plan-pull-a-feature-from-a-release)
- [Change request](#change-request)
- [Production hot fix](#production-hot-fix)

<!-- /TOC -->

## Branches Overview

**TBD: Insert diagram**

| Branch           | Commits Allowed? | Base Branch      | Description    |
| :----------------|:-----------------|:-----------------|:---------------|
| `master`         | NO               | N/A              | What is live in production (**stable**).<br/>A pull request is required to merge code into `master`. |
| `develop`        | YES              | `master`         | The latest state of development (**unstable**). |
| `feature-*`      | YES              | `develop`        | Cutting-edge features (**unstable**). These branches are used for any maintenance features / active development. |
| `release-vX.Y.Z` | NO               | `master`         | A temporary release branch that follows the [semver](http://semver.org/) versioning. This is what is sent to UAT.<br/>A pull request is required to merge code into any `release-vX.Y.Z` branch. |
| `bugfix-*`       | YES              | `release-vX.Y.Z` | Any fixes against a release branch should be made in a bug-fix branch. The bug-fix branch should be merged into the release branch and also into develop. This is one area where we’re deviating from GitFlow. |
| `hotfix-*`       | YES              | `master`         | These are bug fixes against production.<br/>This is used because develop might have moved on from the last published state.<br/>Remember to merge this back into develop and any release branches. |

## Develop a new feature

**TBD: Insert diagram**

1. Make sure your `develop` branch is up-to-date

   ```
   $ git checkout develop
   $ git pull
   ```

1. Create a feature branch based off of `develop`

   ```
   $ git checkout -b feature-new-documentation
   ```

1. Develop the code for the new feature and commit

   ```
   $ git add -A .
   $ git commit -m "Add new documentation files"
   ```

1. When the feature is complete and tested locally, push the feature branch

   ```
   $ git push --set-upstream feature-new-documentation
   ```

1. Navigate to the project on [Github](www.github.com) and open a pull request with the following branch settings:
   * Base: `develop`
   * Compare: `feature-new-documentation`

1. When the pull request was reviewed, merge and close it and delete the `feature-new-documentation` branch.

## Develop multiple features in parallel

There's nothing special about that. Each developer follows the above [Develop a new feature](#develop-a-new-feature) process.

## Create and deploy a release

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
   ```

**TBD: Discuss**
Mike N: Long-lived release branches, yes/no?

1. Tag `master`

   ```
   $ git checkout master
   $ git tag -a vX.Y.Z -m "hotfix-vZ.Y.Z"
   $ git push --tags
   ```

## Change in plan, pull a feature from a release

**TBD: Discuss**
Mike N: That probably means recreating the release branch, unless we have short-lived release branches

## Change request

**TBD: Discuss**
Mike N: That probably means recreating the release branch, unless we have short-lived release branches

## Production hot fix

**TBD: Insert diagram**

1. Make sure your `master` branch is up-to-date

   ```
   $ git checkout master
   $ git pull
   ```

1. Create a hot fix branch based off of `master`

   ```
   $ git checkout -b hotfix-documentation-broken-links
   ```

1. Add a test case to validate the bug, fix the bug and commit

   ```
   $ git add -A .
   $ git commit -m "Fix broken links"
   ```

1. When the fix is complete and tested locally, push the hot fix branch

   ```
   $ git push --set-upstream hotfix-documentation-broken-links
   ```

1. Navigate to the project on [Github](www.github.com) and open a pull request with the following branch settings:
   * Base: `master`
   * Compare: `hotfix-documentation-broken-links`

1. When the pull request was reviewed, merge and close it and delete the `hotfix-documentation-broken-links` branch.

1. Tag `master`

   ```
   $ git checkout master
   $ git tag -a vX.Y.Z -m "hotfix-vZ.Y.Z"
   $ git push --tags
   ```

1. Merge `master` into `develop`. This ensures future releases will contain the hot fix.

   ```
   $ git checkout develop
   $ git merge master
   ```
