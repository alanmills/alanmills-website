# Golang triggering bufio.Scanner io.EOF when using os.Stdin

**Author:** [Alan Mills]
**Date:** [29 October 2021 00:52]
**Tags:** [Go, macOS, bash]
**Status**: Publish 

## Overview

In Chapter 1, Section 1.3 of [The Go Programming Language][1] the `Dup1.go` program makes use of `bufio.Scanner` to `Scan` `os.Stdin` until it detects `io.EOF`.

```go
// Dup1 prints the text of each line that appears more than
// once in the standard input, preceded by its count.
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        counts[input.Text()]++
    }
    // NOTE: ignoring potential errors from input.Err()
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

To test run this program from the command line, run the commands.

```bash
go run dup1.go
Golang
Golang
<ctrl+y>^Y
zsh: suspended  go run dup1.go
fg
[1]  + continued  go run dup1.go
2       Golang
```

### Bash delayed suspend (^Y, Control-Y)

The delayed suspend job control instructs bash to stop the job and return control to bash when the job next attempts to read input from the user terminal `stdin`.  The suspension of the job is detected by `bufio.Scanner.Scan`, resulting in the `for input.Scan()` to return `false`.

### Resume job to foreground job

Therefore when running `fg`, the `Dup1.go` program resumes and outputs the number of duplicate lines.

## References

* [[1]] [The Go Programming Language, Alan A. A. Donovan & Brian W. Kernighan, Addison-Wesley, ISBN-13: 978-0-13-419044-0][1]

[1]: http://www.gopl.io