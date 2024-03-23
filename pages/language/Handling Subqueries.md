
  
  Here is a documentation page in Markdown with the title "Handling Subqueries" based on the code and existing documentation provided:

# Handling Subqueries

The `pql` package uses subqueries to handle complex expressions involving multiple operators. The `splitQueries` function is responsible for splitting a tabular expression into one or more subqueries based on the operators involved.

## Splitting Expressions into Subqueries

The `splitQueries` function takes a tabular expression (`*parser.TabularExpr`) and recursively splits it into subqueries based on the operators present. The function returns a slice of `*subquery` structs, where each struct represents a subquery.

The splitting process works as follows:

1. The function iterates over the operators in the tabular expression.
2. For certain operators like `join`, `sort`, `take`, and `top`, a new subquery is created and appended to the slice of subqueries.
3. The `as` operator is treated similarly to `nil`, but it prevents further operators from being attached to the current subquery.
4. If a new subquery needs to be created, the `chainSubquery` function is called to create a new subquery that either reads from the previous subquery or directly from the data source (if there is no previous subquery).
5. The last subquery in the slice represents the full tabular expression.

## Subquery Struct

The `subquery` struct represents a single subquery and contains the following fields:

- `name`: A unique name for the subquery, generated using the `subqueryName` function.
- `sourceSQL`: The SQL statement that represents the data source for this subquery.
- `op`: The tabular operator associated with this subquery.
- `sort`: The `SortOperator` associated with this subquery (if any).
- `take`: The `TakeOperator` associated with this subquery (if any).

## Writing Subqueries as SQL

The `subquery.write` method is responsible for translating a subquery into its equivalent SQL statement. This method handles various tabular operators and constructs the appropriate SQL syntax based on the operator type and associated expressions.

For example, if the subquery has a `ProjectOperator`, the `write` method generates a `SELECT` statement with the appropriate column projections. If the subquery has a `WhereOperator`, the `write` method adds a `WHERE` clause to the `SELECT` statement.

The `write` method also handles `sort` and `take` operators by appending the necessary `ORDER BY` and `LIMIT` clauses to the SQL statement.

By splitting complex expressions into subqueries and generating SQL statements for each subquery, the `pql` package can effectively translate Pipeline Query Language expressions into equivalent SQL statements.
  
  