# SQL

# Table of contents



# Advanced SQL Window Functions
Window functions were first introduced to standard SQL in 2003. Per the PostgresSQL documentation:

“A window function performs a calculation across a set of table rows that are somehow related to the current row…Behind the scenes, the window function is able to access more than just the current row of the query result.”

Window functions are similar to the aggregation done in the GROUP BY clause. However, rows are not grouped into a single row, each row retains their separate identity. That is, a window function may return a single value for each row.

![Aggregations vs Windows](../misc/sql/aggs_window.png)

**One major advantage of window functions is that it allows you to work with both aggregate and non-aggregate values all at once because the rows are not collapsed together.**

Order of operations in SQL

![Order of operations](../misc/sql/order_of_operations.png)

This is important because based off of this logical order, **window functions are allowed in SELECT and ORDER BY**, but they are not allowed in FROM, WHERE, GROUP BY, or HAVING clauses.

Note: If you really need to have it inside aWHERE clause or GROUP BY clause, you may get around this limitation by using a subquery or a WITH query.

![Window syntax](../misc/sql/window_function_syntax.png)

List of window functions

![Window functions](../misc/sql/window_functions.png)

**Aggregate functions**: we can use these functions to calculate various aggregations such as average, total # of rows, maximum or minimum values, or total sum within each window or partition.

**Ranking functions**: these functions are useful for ranking rows within its partition.

**Value functions**: these functions allow you to compare values from previous or following rows within the partition or the first or last value within the partition.