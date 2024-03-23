
  
  # Writing Expressions in SQL

In the `pql` package, expressions are translated from the Pipeline Query Language (PQL) syntax to SQL. The `writeExpression` function is responsible for this translation. It handles various types of expressions, including qualified identifiers, literals, unary and binary expressions, function calls, and more.

## Qualified Identifiers

Qualified identifiers, such as column names, are translated directly to SQL. However, certain built-in identifiers like `true`, `false`, and `null` are handled specially and translated to their SQL equivalents.

## Literals

Basic literals, such as numbers and strings, are directly translated to their SQL representations.

## Unary Expressions

Unary expressions, such as the unary plus (`+`) and minus (`-`) operators, are translated by applying the operator to the translated expression of the operand.

## Binary Expressions

Binary expressions, such as arithmetic operations (`+`, `-`, `*`, `/`, `%`), comparisons (`<`, `<=`, `>`, `>=`, `=`, `!=`), and logical operations (`and`, `or`), are translated by applying the corresponding SQL operator between the translated expressions of the operands.

## Function Calls

Function calls are handled in two ways:

1. **Known Functions**: For a set of predefined functions, such as `count`, `isnull`, `strcat`, and `tolower`, the package provides dedicated translation logic. These functions are registered in the `initKnownFunctions` function and their translation is implemented in separate functions like `writeCountFunction` and `writeStrcatFunction`.

2. **Unknown Functions**: If a function is not recognized as a known function, it is passed through to the underlying SQL engine. This allows the usage of the full set of functions provided by the underlying database system.

## Error Handling

If an expression cannot be translated due to an unsupported construct or incorrect usage, a `compileError` is returned. The `compileError` struct contains the source PQL expression, the position (span) of the error within the expression, and the error message. This error can be formatted with line and column numbers for better error reporting.

Overall, the `pql` package provides a convenient way to translate PQL expressions into SQL expressions, supporting various operators, expressions, and scalar functions, while handling errors and producing valid SQL syntax for the underlying database system.
  
  