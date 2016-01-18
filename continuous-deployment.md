# Continuous Deployment

Use this strategy for projects where features get deployed as soon as they're ready.

We follow the [**GitHub flow**](https://guides.github.com/introduction/flow/)
workflow as closely as possible. This page showcases common development scenarios
and how to deal with them from a branching point of view.

- [Branches Overview](#branches-overview)
- [Develop a new feature](#develop-a-new-feature)
- [Develop multiple features in parallel](#develop-multiple-features-in-parallel)
- [Create and deploy a release](#create-and-deploy-a-release)
- [Change in plan, pull a feature from a release](#change-in-plan-pull-a-feature-from-a-release)
- [Change request](#change-request)
- [Production hot fix](#production-hot-fix)

## Branches Overview

**TBD: Insert diagram**

| Branch           | Commits Allowed? | Base Branch      | Description    |
| :----------------|:-----------------|:-----------------|:---------------|
| `master`         | NO               | N/A              | What is live in production (**stable**).<br/>A pull request is required to merge code into `master`. |
| `feature-*`      | YES              | `master`        | Cutting-edge features (**unstable**). These branches are used for any maintenance features / active development. |
| `hotfix-*`       | YES              | `master`         | These are bug fixes against production.<br/> |

## Develop a new feature

**TBD: Insert diagram**

1. Make sure your `master` branch is up-to-date.

   ```
   $ git checkout master
   $ git pull
   ```

1. Create a feature branch based off of `master`.

   ```
   $ git checkout -b feature-new-documentation
   ```

1. Develop the code for the new feature and commit as you go.

   ```
   $ ... make changes
   $ git add -A .
   $ git commit -m "Add new documentation files"
   $ ... make more changes
   $ git add -A .
   $ git commit -m "Fix some spelling errors"
   ```

1. As a final step before creating a pull request, be sure to update your branch
from the `master` branch. This makes sure the code you are merging into `master`
is exactly the same as what you're testing.

   ```
   $ git checkout master
   $ git pull
   $ git checkout feature-new-documentation
   $ git merge master
   ```

1. When the feature is complete and tested locally, push the feature branch.

   ```
   $ git push --set-upstream feature-new-documentation
   ```

1. Navigate to the project on [Github](www.github.com) and open a pull request
with the following branch settings:
   * Base: `master`
   * Compare: `feature-new-documentation`

1. When the pull request has been reviewed and ![+1'd](images/plus1.png)
, merge and close it and then delete the `feature-new-documentation` branch.

1. Deploy `master` to a staging environment to verify (_some teams have this
    automated, some prefer a manual deploy through convention, either is fine_).

1. If everything is good in staging, promote it to production and you're done.
If not, roll back production to the previous release and return to Step 1.

## Develop multiple features in parallel

There's nothing special about that. Each developer follows the above
[Develop a new feature](#develop-a-new-feature) process.

*One caveat is to make sure that before you merge to `master` you update your
branch from `master`. This ensures that you have all changes that have been merged
into `master` since you created your branch. If you don't do this, git may still
be able to automatically merge your code, but there is no guarantee that the
code you tested in your branch is exactly what you deploy from production. As a
result, your final step before you merge to `master` should always
include merging from `master` doing a final test and then merging.*

## Production hot fix

*In rare situations you may need to get a fix into production fast! Use this
workflow to push a hotfix to production when you can't spare the time to
follow the standard 'Develop a feature' workflow.*

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
