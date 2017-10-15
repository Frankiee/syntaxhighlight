# syntaxhighlight

Package syntaxhighlight provides syntax highlighting for code. It currently uses a language-independent lexer and performs decently on JavaScript, Java, Ruby, Python, Go, and C.

The main [`AsHTML(src []byte) ([]byte, error)`](https://sourcegraph.com/sourcegraph.com/sourcegraph/syntaxhighlight@master/.GoPackage/sourcegraph.com/sourcegraph/syntaxhighlight/.def/AsHTML) function outputs HTML that uses the same CSS classes as [google-code-prettify](https://code.google.com/p/google-code-prettify/), so any stylesheets for that should also work with this package.

**[Documentation on Sourcegraph](https://sourcegraph.com/github.com/sourcegraph/syntaxhighlight)**

[![Build Status](https://travis-ci.org/sourcegraph/syntaxhighlight.png?branch=master)](https://travis-ci.org/sourcegraph/syntaxhighlight)
[![status](https://sourcegraph.com/api/repos/github.com/sourcegraph/syntaxhighlight/badges/status.png)](https://sourcegraph.com/github.com/sourcegraph/syntaxhighlight)

## Installation

```
go get -u github.com/sourcegraph/syntaxhighlight
```
First you should install the golang evironment, you can download it [here](https://golang.org/dl) or you can follow the [getting started](https://golang.org/doc/install)

Remember you should set the environment variables correctly (GOPATH and PATH)

## Example usage

The function [`AsHTML(src []byte, options ...Option) ([]byte, error)`](https://sourcegraph.com/sourcegraph.com/sourcegraph/syntaxhighlight@master/.GoPackage/sourcegraph.com/sourcegraph/syntaxhighlight/.def/AsHTML) returns an HTML-highlighted version of `src`. The input source code can be in any language; the lexer is language independent. An `OrderedList()` option can be passed to produce an `<ol>...</ol>`-wrapped list to display line numbers.

321
```go
value, ok := cache.Lookup(key)
if !ok {
    // ...cache[key] does not exist...
}
```
123


## Contributors

* [Quinn Slack](https://sourcegraph.com/sqs)

Contributions are welcome! Submit a pull request on GitHub.
