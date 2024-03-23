
  
  # Rewriting Simple Join Conditions

The `pql` package handles simple join conditions involving a single identifier in a special way. The `rewriteSimpleJoinCondition` function is responsible for rewriting these simple join conditions to conform with the Clickhouse SQL dialect.

Clickhouse requires a basic equality comparison between the left and right columns when joining tables. It does not support more complex expressions in the join condition.

The `rewriteSimpleJoinCondition` function checks if the provided expression is a single qualified identifier (an identifier without any special quoting). If this is the case, it rewrites the expression to a binary expression that compares the left and right table aliases with the identifier.

For example, if the expression is `id`, it will be rewritten as `$left.id = $right.id`. This ensures that the generated SQL follows the Clickhouse syntax for join conditions.

If the expression is not a simple qualified identifier, it is left unchanged.

This rewriting is necessary to handle join conditions correctly when generating SQL for Clickhouse or any other database that has similar requirements for join conditions.

## Example

```go
// Original PQL expression
users | join workspaces on workspace_id

// Rewritten join condition
$left.workspace_id = $right.workspace_id
```

In this example, the simple join condition `workspace_id` is rewritten to compare the `workspace_id` columns from the left and right tables using the `$left` and `$right` aliases.
  
  