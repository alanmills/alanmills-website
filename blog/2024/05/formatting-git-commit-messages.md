# Formatting Git commit messages

**Author:** [Alan Mills]
**Date:** [31 May 2024 22:58]
**Tags:** [Git]
**Status**: Draft

```bash
<ticket-id>: <type>(<scope>): <subject>
<message>
```

## ticket-id

In most corporate environments projects are managed with Jira, Azure DevOps,GitHub Issues and lots of other work tracking systems.  In these environments commit messages normally start with the work reference so the change can be linked back e.g. `PRJFUN-9217`.

## type

`feat` for a new feature for the user, not a new feature for build script. Such commit will trigger a release bumping a MINOR version.

`fix` for a bug fix for the user, not a fix to a build script. Such commit will trigger a release bumping a PATCH version.

`chore` A code change that external user won't see (eg: change to .gitignore file or .prettierrc file)

`perf` for performance improvements. Such commit will trigger a release bumping a PATCH version.

`docs` for changes to the documentation.

`style` for formatting changes, missing semicolons, etc.

`refactor` for refactoring production code, e.g. renaming a variable.

`test` for adding missing tests, refactoring tests; no production code change.

`build` for updating build configuration, development tools or other changes irrelevant to the user.

## scope

## message

## Reference

* [Karma: Git Commit Msg][karma]
* [Git commit message convention that you can follow!][ishan]
* [commitlint - Lint commit messages][commitlint]
* [Husky - enhances your commits and more][husky]

[ishan]: https://dev.to/ishanmakadia/git-commit-message-convention-that-you-can-follow-1709
[karma]:http://karma-runner.github.io/6.4/dev/git-commit-msg.html
[commitlint]: https://commitlint.js.org
[husky]: https://typicode.github.io/husky/
