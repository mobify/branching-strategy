# Mobify Branching Strategy

This document represents Mobify's current branching and release strategy. It provides a brief overview of the two release models that we use: [release-based](release-deployment.md) and [continuous delivery](continuous-deployment.md).

Each workflow tries to make things as simple as possible while still being flexible enough to work for all teams at Mobify.

At the end of each document is a list of common scenarios you will encounter and how Mobify's branching strategies apply.

## What is the purpose of this repository?

As Mobify continues to grow and expand its operations globally, consistency across all teams and partners is a key
focus. The more aligned all Mobify projects are, the more productive everyone will be.

This repository and its documentation outline:
* How we develop features
* When and how we create a release
* What to do when a hot fix is required
* Anything else related to our branching strategy

### What the repository is not:

* Written in stone. Pull requests are welcome and we will update this document with lessons learned and improvements as we go
* The one and only way to work on projects. There are always certain edge cases where things have to be tweaked, but don't let tweaking become the norm

## Project Types

Depending on the type of project, one of two branching strategies is used:

### 1) Continuous Deployment

Use this strategy for projects where features get deployed as soon as they're ready.

[Learn more](./continuous-deployment.md)

### 2) Release Deployment

Use this strategy for projects where features get bundled into a release and then
deployed together.

[Learn more](./release-deployment.md)

## Branch Naming

Branch naming is left mostly up to the discretion of the person creating the branch
with a few exceptions. `master` and `develop` are always named exactly that. When a
feature/bugfix is related to a JIRA ticket we prefer that the branch name start with
the ticket number (eg. `hyb-545-add-headerbar`). Hotfix and release branches should
follow a pattern defined in the project type documents (ie. `release-vX.Y.Z` or
(eg. `hotfix-hyb-244-fix-db-connection-code`), but feature and bugfix branches
**don not** need a common prefix (ie. `feature-*`).

Branch names should use dashes to separate words of the name and should avoid any
uppercase letters.

Other than that, choose names that are descriptive and concise. You don't need a branch
name that is a novel because most branches should be relatively short-lived (hours to
days, not weeks).

## Anti-Patterns

After reading all of the above, none of the [Anti-Patterns](antipatterns.md) should
come as a surprise.
