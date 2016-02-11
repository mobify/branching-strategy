# Anti-Patterns

This page lists some anti-patterns we want to avoid at all cost. Seriously, don't
even think of doing anything you see below.

- [Don't develop a feature in `develop` or `master`](#dont-develop-a-feature-in-develop-or-master)
- [Don't fix a bug in `master`](#dont-fix-a-bug-in-master)
- [Don't merge your pull requests without a +1](#dont-merge-your-pull-requests-without-a-1)

## Don't develop a feature in `develop` or `master`

**Instead**: Create a feature branch off of `develop` or `master`. When the
feature is developed and tested, create a pull request.

**Why?**: All code changes require a code review and verification by our QA team.
By opening a pull request, you signalize to the rest of the team that your code
is ready to be reviewed and tested.

* * *

## Don't fix bugs in `master`

**Instead**: Create a hotfix branch off of `master`. Write a test case, fix the bug
and create a pull request.

**Why?**: Production bug fixes are super critical and it's especially important
to properly review them. If nobody is around to review, quickly escalate to your
lead.

**But... my boss or project manager stands next to me** and says, "_Deploy it already!_"
Kindly point them to this document, ask for help to escalate and find someone who can review
the pull request.

* * *

## Don't merge your pull requests without a +1

**Instead**: Ask a team member for a code review and to +1 your changes.

**Why?**: _Nobody is perfect._ Having said that, we always want to make sure at
least one other team member reviews our code. Performance, readability, bugs,
memory leaks, etc can all be impacted by code changes and sharing the responsibility
to think about all that with a team member takes pressure off your shoulders.

* * *
