
  
  # Supported Functions

## Known Functions

The `pql` package supports a set of built-in scalar functions, such as `count`, `isnull`, `strcat`, and `tolower`. These functions are registered in the `initKnownFunctions` function and their translation is implemented in separate functions like `writeCountFunction` and `writeStrcatFunction`.

The following scalar functions are implemented within `pql`:

- `not`
- `now`
- `isnull` and `isnotnull`
- `strcat`
- `iff`/`iif`
- `count`
- `countif`
- `tolower`
- `toupper`

### Function Implementation

The implementation of each known function follows a similar pattern. The `write` function first checks the number of arguments provided to the function call. If the number of arguments is incorrect, it returns a `compileError` with an appropriate error message.

Next, the `write` function generates the SQL syntax for the function, often by calling `writeExpression` or `writeExpressionMaybeParen` to translate the function arguments into SQL expressions.

Some functions, such as `strcat` or `iff`, may require additional logic to handle specific cases or perform additional operations on the arguments.

### Error Handling

If an error occurs during the translation of a function call, the `write` function returns a `compileError`. The `compileError` struct contains the source PQL expression, the span (position) of the error within the expression, and the error message.

When a `compileError` is returned, it can be formatted with line and column numbers for better error reporting.

## Unknown Functions

If a function is not recognized as a known function, it is passed through to the underlying SQL engine. This allows the usage of the full set of functions provided by the underlying database system.
  
  