| [Home](README.md) ▸ **Continuous Deployment** |
|-----|

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
- [Develop in a platform repo](#platform-repo)

## Branches Overview

![Github flow workflow](images/continuous-overview.png)

| Branch  | Protected?  | Base Branch      | Description    |
| :-------|:------------|:-----------------|:---------------|
| `master`| YES         | N/A              | What is live in production (**stable**).<br/>A pull request is required to merge code into `master`. |
| feature | NO          | `master`         | Cutting-edge features (**unstable**). These branches are used for any maintenance features / active development. |
| `hotfix-*` | NO       | `master`         | These are bug fixes against production.<br/> |

## Develop a new feature

![Hotfix **use rarely**](images/continuous-new-feature.png)

1. Create a feature branch based off of `master`.

   ```
   $ git checkout master
   $ git checkout -b MYTEAM-123-new-documentation
   $ git push --set-upstream MYTEAM-123-new-documentation
   ```

1. Develop the code for the new feature and commit as you go.

   ```
   $ ... make changes
   $ git add -A .
   $ git commit -m "Add new documentation files"
   $ ... make more changes
   $ git add -A .
   $ git commit -m "Fix some spelling errors"
   $ git push
   ```

1. Navigate to the project on [Github](www.github.com) and open a pull request
with the following branch settings:
   * Base: `master`
   * Compare: `MYTEAM-123-new-documentation`

1. When the pull request has been reviewed and ![+1'd](images/plus1.png)
, merge and close it and then delete the `MYTEAM-123-new-documentation`
branch. This can all be done from the Github pull-request page.

1. Deploy `master` to a staging environment to verify (_some teams have this
    automated, some prefer a manual deploy with some conventions, either is fine_).

1. If everything is good in staging, promote it to production and you're done.
If not, roll back production to the previous release and return to Step 1.

## Develop multiple features in parallel

There's nothing special about that. Each developer follows the above
[Develop a new feature](#develop-a-new-feature) process.

During development, make sure to update from `master` often so that when you
get ready to complete your feature you don't have to deal with large code
conflicts.

## Production hot fix

*In rare situations you may need to get a fix into production fast! Use this
workflow to push a hotfix to production when you can't spare the time to
follow the standard 'Develop a feature' workflow.*

![Hotfix **use rarely**](images/continuous-hotfix.png)

*This is very similar to how we [Develop a new feature](#develop-a-new-feature)
described above.*

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
   *When doing a hotfix you should at _least_ pair on the fix with somebody or
   review it in person with one other engineer before releasing it. We're
   running without training wheels here and want to do our best not to have to
   do a stream of hotfixes in production.*
   ```
   ... add test, fix bug, verify
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

## Develop in a platform repo

A platform repo contains both Progressive Mobile App and Progressive Mobile Web
source code.

The development process is the same as outlined above in
[Develop a new feature](#develop-a-new-feature).

**However**, branches have to be prefixed based on the following rules:
* `app-`, for changes mainly touching Progressive Mobile App code.
* `web-`, for changes mainly touching Progressive Mobile Web code.
* `platform-`, for changes touching both Progressive Mobile App and Web code.