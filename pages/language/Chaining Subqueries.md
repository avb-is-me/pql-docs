
  
  # Chaining Subqueries

The `pql` package implements a mechanism to chain subqueries, which allows for complex expressions to be broken down into smaller, more manageable subqueries. This is achieved through the `chainSubquery` function.

## The `chainSubquery` Function

The `chainSubquery` function is responsible for creating a new subquery that either reads from the previous subquery or directly from the data source, depending on the situation. Here's how it works:

```go
func chainSubquery(dst []*subquery, dstStart int, src parser.TabularDataSource) (*subquery, error) {
    sub := &subquery{
        name: subqueryName(len(dst)),
    }
    sb := new(strings.Builder)
    if len(dst) > dstStart {
        quoteIdentifier(sb, dst[len(dst)-1].name)
    } else {
        if err := dataSourceSQL(sb, src); err != nil {
            return nil, err
        }
    }
    sub.sourceSQL = sb.String()
    return sub, nil
}
```

The `chainSubquery` function creates a new `subquery` struct and sets its `name` field using the `subqueryName` function. Then, it constructs the `sourceSQL` field for the new subquery. If there is a previous subquery, it quotes the name of the previous subquery and uses it as the source for the new subquery. Otherwise, it uses the data source (`src`) as the source.

This mechanism allows the `pql` package to handle complex expressions by breaking them down into smaller subqueries. Each subquery represents a part of the overall expression, and the final subquery represents the full expression.

## Using Subqueries in the Compilation Process

The `splitQueries` function is responsible for splitting a tabular expression into one or more subqueries based on the operators involved. It calls `chainSubquery` when it needs to create a new subquery that is chained to the previous subquery or the data source.

The `subquery.write` method is then responsible for translating each subquery into its equivalent SQL statement, taking into account the operators and expressions associated with that subquery.

By chaining subqueries and generating SQL statements for each subquery, the `pql` package can effectively translate complex Pipeline Query Language expressions into equivalent SQL statements.
  
  