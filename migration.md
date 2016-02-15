> [Home](README.md) ▸ **Migration**

# Migration

Migrating projects to the new branching strategies is a simple task outlined below.

**Note**: The process applies to [release deployment](release-deployment.md) projects.
Any legacy continuous deployment project simply follows the
instructions [here](continuous-deployment.md).

## Migration Process

1. Determine which branch is deployed to production.
    * For illustration purposes, let's say it's `current-prod`
1. Merge `current-prod` into `master`

   ```
   $ git checkout master
   $ git merge current-prod
   ```

1. Create a `develop` branch off of `master` if none exists

   ```
   $ git checkout master
   $ git checkout -b develop
   ```

1. Notify your team branching & release Czar so that he or she can protect the following branches:
    * `master`
    * `develop`

## Protected Branches

[Github](www.github.com) offers a feature called
[Protected Branches](https://help.github.com/articles/defining-the-mergeability-of-pull-requests/).
We leverage this to make sure our core branches are safe.

To protect a branch, follow these instructions:

1. Navigate to your Github repository
1. Click on **Settings** and then on **Branches** in the left-hand navigation
1. In the **Protected branches** section, configure both `master` and `develop` branches.
