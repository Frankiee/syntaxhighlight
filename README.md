# syntaxhighlight

go
```go
var i interface{} = "hello"

s, ok := i.(string)
fmt.Println(s, ok)     // `hello true`

f, ok := i.(float64)
fmt.Println(f, ok)     // `0 false`
```
og
