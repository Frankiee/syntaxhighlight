# syntaxhighlight

go
```go
package main

import "fmt"
import "time"

func main() {
    go spinner(100 * time.Millisecond)
    const n = 45
    fibN := fib(n) // slow
    fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}

func spinner(delay time.Duration) {
    for {
        for _, r := range `-\|/` {
            fmt.Printf("\r%c", r)
            time.Sleep(delay)
        }
    }
}


func fib(x int) int {
    if x < 2 {
      return x
    }
    return fib(x-1) + fib(x-2)
}
```
og


python
```python
##############################
# S3 Buckets
##############################
resource "aws_s3_bucket" "cs-monitoring-prod" {
    bucket = "cs-monitoring-prod"
    acl = "private"

    tags {
        Environment = "prod"
    }
}

resource "aws_s3_bucket" "cs-monitoring-stage" {
    bucket = "cs-monitoring-stage"
    acl = "private"

    tags {
        Environment = "stage"
    }
}
```
python
