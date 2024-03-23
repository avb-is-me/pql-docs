
  
  # Handling Errors and Spans

The `pql` package uses the `compileError` struct to represent errors that occur during the compilation process. This struct includes the source PQL expression, the span (position) of the error within the expression, and the error message.

```go
type compileError struct {
    source string
    span   parser.Span
    err    error
}
```

When an error occurs during compilation, a `compileError` is returned, which can be formatted with line and column numbers for better error reporting. The `Error()` method provides a string representation of the error, including the line and column numbers if the span is valid.

```go
func (e *compileError) Error() string {
    if !e.span.IsValid() {
        return e.err.Error()
    }
    line, col := linecol(e.source, e.span.Start)
    return fmt.Sprintf("%d:%d: %s", line, col, e.err.Error())
}
```

The `Unwrap()` method allows the `compileError` to be unwrapped to retrieve the underlying error.

```go
func (e *compileError) Unwrap() error {
    return e.err
}
```

The `linecol` function is a helper function that calculates the line and column numbers for a given position in the source string.

```go
func linecol(source string, pos int) (line, col int) {
    line, col = 1, 1
    for _, c := range source[:pos] {
        switch c {
        case '\n':
            line++
            col = 1
        case '\t':
            const tabWidth = 8
            tabLoc := (col - 1) % tabWidth
            col += tabWidth - tabLoc
        default:
            col++
        }
    }
    return
}
```

The `compileError` struct is used throughout the `pql` package to handle errors that occur during the compilation process, such as when encountering unsupported constructs or incorrect usage. By including the span information, the package can provide more informative error messages with line and column numbers, making it easier to debug and understand the errors in the context of the PQL expression.
  
  