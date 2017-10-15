# syntaxhighlight

go
```go
package main

import "fmt"

type Dog struct {
}

func (d Dog) Say() {
    fmt.Println("Woof!")
}

func main() {
    d := &Dog{}
    d.Say()    // print `Woof!`
}
```
og
