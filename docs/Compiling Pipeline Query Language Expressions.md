# Compiling Pipeline Query Language Expressions

The `pql` package provides a way to compile Pipeline Query Language (PQL) expressions into equivalent SQL statements. The main entry point for this is the `Compile` function:

```go
func Compile(source string) (string, error)
```

The `Compile` function takes a PQL expression as a string and returns the equivalent SQL statement as a string. If there is an error during compilation, an error is returned.

## Compilation Process

The compilation process involves several steps:

1. **Parsing**: The PQL expression is parsed into an abstract syntax tree (AST) using the `parser.Parse` function.
2. **Splitting into Subqueries**: The AST is analyzed, and the expression is split into multiple subqueries based on operators like `join`, `sort`, `take`, and `as`. This is done by the `splitQueries` function.
3. **Writing SQL**: Each subquery is then translated into an equivalent SQL statement by the `subquery.write` function. This function handles different operators and constructs the appropriate SQL syntax.
4. **Handling Expressions**: Expressions within the PQL statement are translated into SQL using the `writeExpression` function. This function handles various expression types, such as identifiers, literals, unary and binary expressions, function calls, and more.
5. **Generating Output**: The resulting SQL statements for all subqueries are combined into a single SQL statement using the Common Table Expression (CTE) syntax if needed. The final SQL statement is returned by the `Compile` function.

## Handling Scalar Functions

The `pql` package supports a set of built-in scalar functions, such as `count`, `isnull`, `strcat`, and `tolower`. These functions are handled by the `writeExpression` function and translated into their equivalent SQL syntax.

If a function is not recognized as a built-in function, it is passed through to the underlying SQL engine. This allows the usage of the full set of functions provided by the underlying database system.

## Handling Tabular Operators

The `pql` package supports various tabular operators like `project`, `extend`, `summarize`, `where`, and `join`. Each operator is handled by the `subquery.write` function, which constructs the appropriate SQL syntax for that operator.

For example, the `join` operator is handled by generating a `JOIN` clause in SQL, with the appropriate join type (e.g., `INNER JOIN`, `LEFT OUTER JOIN`) and join condition.

## Error Handling

The `pql` package uses the `compileError` struct to represent errors that occur during the compilation process. This struct includes the source PQL expression, the span (position) of the error within the expression, and the error message.

When an error occurs during compilation, a `compileError` is returned, which can be formatted with line and column numbers for better error reporting.

Overall, the `pql` package provides a convenient way to translate PQL expressions into SQL statements, with support for various operators, expressions, and scalar functions. The compilation process is designed to handle different scenarios and produce valid SQL syntax for the underlying database system.
  
  
