# syntaxhighlight

go
```go
var i interface{} = "hello"

s := i.(string)
fmt.Println(s)    // `hello`

f = i.(float64)   // panic: interface conversion: interface {} is string, not float64
fmt.Println(f)
```
og
