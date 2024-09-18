# Rename multiple files using Unix command line

**Author:** [Alan Mills]
**Date:** [18 September 2024 19:19]
**Tags:** [Bash, mv, sed, regex]
**Status**: Publish

Sometimes, you want to rename a set of files in a folder that follows a naming pattern.  Using bash, mv, and sed, you can rename the files.

## TL;DR

```bash
for f in start_file_pattern*; do mv "$f" $(echo "$f" | sed 's/^start_file_pattern/updated_file_pattern/g'); done
```

## Explanation of how it works

This command is comprised of multiple parts:

1. `for f in start_file_pattern*; do STEP_2 done` uses the search pattern to itterate over.  start_file_pattern should match the common start/middle/end pattern for the files you wish to rename.
2. `mv "$f $(STEP_3)` uses the move command to move the original file `$f` to the new name `$(STEP_3)
3. `$(echo "$f" | sed 's/^start_file_pattern/updated_file_pattern/g')` uses echo and sed to rename the matching file using a regular expression.  In this example, the pattern `^start_file_pattern` matches the start of the matched filed name.

## Example - renaming Go source files

In this example, there are a number of files associated with a simple Golang project that have many files starting with `httptest`.  After the move, the Golang source files will start with `integrationtest`.

### Files at the start

```bash
ls

benchstat.old.txt  cmd            coverage.out  httptest_solver_client.go       httptest_solver.go      httptest_solver_test.go
benchstat.txt      coverage.html  go.mod        httptest_solver_client_test.go  httptest_solver_server  Makefile
```

### Rename the files

```bash
for f in httptest*.go; do mv "$f" $(echo "$f" | sed 's/^httptest/integrationtest/g'); done
```

### Files at the end

```bash
ls

benchstat.old.txt  coverage.html  httptest_solver_server                 integrationtest_solver.go
benchstat.txt      coverage.out   integrationtest_solver_client.go       integrationtest_solver_test.go
cmd                go.mod         integrationtest_solver_client_test.go  Makefile
```
