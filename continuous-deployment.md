# Continuous Deployment

Use this strategy for projects where features get deployed as soon as they're ready.

We follow the **GitHub flow** workflow as closely as possible. This page showcases common development scenarios and how to deal with them from a branching point of view.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

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
| `feature-*`      | YES              | `master`        | Cutting-edge features (**unstable**). These branches are used for any maintenance features / active development. |
| `hotfix-*`       | YES              | `master`         | These are bug fixes against production.<br/> |

## Develop a new feature

**TBD: Insert diagram**

1. Make sure your `master` branch is up-to-date
   ```
   $ git checkout master
   $ git pull
   ```
1. Create a feature branch based off of `master`
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
   * Base: `master`
   * Compare: `feature-new-documentation`
1. When the pull request was reviewed, merge and close it and delete the `feature-new-documentation` branch.
1. Tag `master`
   ```
   $ git checkout master
   $ git tag -a vX.Y.Z -m "hotfix-vZ.Y.Z"
   $ git push --tags
   ```

## Develop multiple features in parallel

There's nothing special about that. Each developer follows the above [Develop a new feature](#develop-a-new-feature) process.

## Create and deploy a release

**TBD: Discuss**
Mike N: I can see two scenarios, with or without continuous deployment set up

## Change in plan, pull a feature from a release

**TBD: Discuss**
Mike N: Is that as simple as reverting the PR merge when the feature was added? Should each PR just be 1 commit (using rebase)?

## Change request

**TBD: Discuss**
Mike N: Same as [Develop a new feature](#develop-a-new-feature)

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
