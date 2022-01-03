# How to run a Github CICD Workflow

**Author:** [Alan Mills]
**Date:** [3 January 2022 18:04]
**Tags:** [Git, Github, CICD]
**Status**: Draft

## Overview

How to run a Continuous Delivery workflow using Github.

Topics

* Git Branch Naming
* Tags
* Github Issues
* Github Actions
* Local changes and commit messages
* Pull Requests (PRs)

## Local changes and commit messages

When working on the code base with a Distributed Version Control (DVC) system like Git, we need to make small changes and integrate these back to master as quickly as possible to reduce integration pain for everyone on the team.

Every project works with version control and commit messages differently.  Some of the examples I have seen over time that have different issues are:

* Messages without context, e.g. `Fixed it!`,
* A list of issue tracking system references, e.g. `PRJ-123`, 
* A single commit for months of work by a development team `Q2 Release changes`,
* A verbose commit messages, e.g. `This change fixes the duplicate products stored in the DB following an HTTP POST using the Products Catalogue API.  The issue with the Product Catalog API is the retry logic timeout was shorter than the retry logic in the Data Tier when the On-demand DB instance was cold starting.  These differences in timeout resulted in the first (or more) DB insert completing successfully.  The Product Catalogue API handler would drop the call to the Data Layer Handler; however, the Circuit Breaker logic retries multiple times, each queued by the Data Layer Handler.  So, when the DB warmed up, all of the insert call(s) from the Product Catalogue Handler was completed successful, resulting in multiple identical products in the DB with different UUIDs`

The goals for the git log is:

1. Atomic change (change one thing at a time)
2. Type of change
3. Status and reference to the Github issue, this allows us to get more information on the change (if required)
4. A concise statement of the purpose of the commit.

```Git
Summary (less than 70 characters - Git recommends 50, but Github displays 70)

Body
```

### Example Git commit message

```Git
Bug #215: PCA inserts duplicate products on cold startup

This change fixes the duplicate products stored in the DB following an HTTP POST using the Products Catalogue API.  The issue with the Product Catalog API is the retry logic timeout was shorter than the retry logic in the Data Tier when the On-demand DB instance was cold starting.  These differences in timeout resulted in the first (or more) DB insert completing successfully.  The Product Catalogue API handler would drop the call to the Data Layer Handler; however, the Circuit Breaker logic retries multiple times, each queued by the Data Layer Handler.  So, when the DB warmed up, all of the insert call(s) from the Product Catalogue Handler was completed successful, resulting in multiple identical products in the DB with different UUIDs`.
```

## References

* [git-commit, Discussion](https://git-scm.com/docs/git-commit#_discussion)
* [Git Branch Naming Convention](https://dev.to/couchcamote/git-branching-name-convention-cch)
* [Git commit message convention that you can follow!](https://dev.to/i5han3/git-commit-message-convention-that-you-can-follow-1709)
* [Git Log](https://www.toolsqa.com/git/git-log/)
* [Github Issues](https://docs.github.com/en/issues)
* [Github Issues - Link PR to issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
* [Github Actions](https://docs.github.com/en/actions)