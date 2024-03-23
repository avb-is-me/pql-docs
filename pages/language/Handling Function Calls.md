
  
  # Handling Function Calls

The `pql` package supports a set of built-in scalar functions, such as `count`, `isnull`, `strcat`, and `tolower`. These functions are handled by the `writeExpression` function and translated into their equivalent SQL syntax.

If a function is not recognized as a built-in function, it is passed through to the underlying SQL engine. This allows the usage of the full set of functions provided by the underlying database system.

## Known Functions

Known functions are registered in the `initKnownFunctions` function. Each function has an associated `functionRewrite` struct that contains a `write` function responsible for translating the function call into SQL.

The `write` function is called by the `writeExpression` function when it encounters a known function call. It receives the `exprContext` and the `CallExpr` representing the function call, and it generates the appropriate SQL syntax.

## Function Implementation

The implementation of each known function follows a similar pattern. The `write` function first checks the number of arguments provided to the function call. If the number of arguments is incorrect, it returns a `compileError` with an appropriate error message.

Next, the `write` function generates the SQL syntax for the function, often by calling `writeExpression` or `writeExpressionMaybeParen` to translate the function arguments into SQL expressions.

Some functions, such as `strcat` or `iff`, may require additional logic to handle specific cases or perform additional operations on the arguments.

## Error Handling

If an error occurs during the translation of a function call, the `write` function returns a `compileError`. The `compileError` struct contains the source PQL expression, the span (position) of the error within the expression, and the error message.

When a `compileError` is returned, it can be formatted with line and column numbers for better error reporting.

## Adding New Functions

To add a new function, follow these steps:

1. Add a new entry to the function table in `initKnownFunctions`.
2. Create a callback function that writes the SQL you want.
3. Callbacks can inspect the full AST of their function call and can use `writeExpressionMaybeParen` to translate their arguments.

By handling function calls in this manner, the `pql` package can effectively translate PQL function calls into equivalent SQL statements, while also allowing the usage of functions provided by the underlying database system.
  
  