| Operator            | Condition                                            | SQL Example                    |
| ------------------- | ---------------------------------------------------- | ------------------------------ |
| \=, !=, < <=, >, >= | Standard numerical operators                         | col\_name != 4                 |
| BETWEEN … AND …     | Number is within range of two values (inclusive)     | col\_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) | col\_name NOT BETWEEN 1 AND 10 |
| IN (…)              | Number exists in a list                              | col\_name IN (2, 4, 6)         |
| NOT IN (…)          | Number does not exist in a list                      | col\_name NOT IN (1, 3, 5)     |

<br/><br/>

| Operator     | Condition                                                                                             | Example                                                              |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `=`          | Case sensitive exact string comparison (notice the single equals)                                     | `col_name = "abc"`                                                   |
| `!= or <>`   | Case sensitive exact string inequality comparison                                                     | `col_name != "abcd"`                                                 |
| `LIKE`       | Case insensitive exact string comparison                                                              | `col_name LIKE "ABC"`                                                |
| `NOT LIKE`   | Case insensitive exact string inequality comparison                                                   | `col_name NOT LIKE "ABCD"`                                           |
| `%`          | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | `col_name LIKE "%AT%"` (matches "AT", "ATTIC", "CAT" or even "BATS") |
| `_`          | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)                    | `col_name LIKE "AN_"` (matches "AND", but not "AN")                  |
| `IN (…)`     | String exists in a list                                                                               | `col_name IN ("A", "B", "C")`                                        |
| `NOT IN (…)` | String does not exist in a list                                                                       | `col_name NOT IN ("D", "E", "F")`                                    |

<br/><br/>

| Function                 | Description                                                                                                                                                                                     |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| COUNT(\*), COUNT(column) | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| MIN(column)              | Finds the smallest numerical value in the specified column for all rows in the group.                                                                                                           |
| MAX(column)              | Finds the largest numerical value in the specified column for all rows in the group.                                                                                                            |
| AVG(column)              | Finds the average numerical value in the specified column for all rows in the group.                                                                                                            |
| SUM(column)              | Finds the sum of all numerical values in the specified column for the rows in the group.                                                                                                        |

<br/><br/>

Query order of execution
1. FROM and JOINs
The FROM clause, and subsequent JOINs are first executed to determine the total working set of data that is being queried. This includes subqueries in this clause, and can cause temporary tables to be created under the hood containing all the columns and rows of the tables being joined.

2. WHERE
Once we have the total working set of data, the first-pass WHERE constraints are applied to the individual rows, and rows that do not satisfy the constraint are discarded. Each of the constraints can only access columns directly from the tables requested in the FROM clause. Aliases in the SELECT part of the query are not accessible in most databases since they may include expressions dependent on parts of the query that have not yet executed.

3. GROUP BY
The remaining rows after the WHERE constraints are applied are then grouped based on common values in the column specified in the GROUP BY clause. As a result of the grouping, there will only be as many rows as there are unique values in that column. Implicitly, this means that you should only need to use this when you have aggregate functions in your query.

4. HAVING
If the query has a GROUP BY clause, then the constraints in the HAVING clause are then applied to the grouped rows, discard the grouped rows that don't satisfy the constraint. Like the WHERE clause, aliases are also not accessible from this step in most databases.

5. SELECT
Any expressions in the SELECT part of the query are finally computed.

6. DISTINCT
Of the remaining rows, rows with duplicate values in the column marked as DISTINCT will be discarded.

7. ORDER BY
If an order is specified by the ORDER BY clause, the rows are then sorted by the specified data in either ascending or descending order. Since all the expressions in the SELECT part of the query have been computed, you can reference aliases in this clause.

8. LIMIT / OFFSET
Finally, the rows that fall outside the range specified by the LIMIT and OFFSET are discarded, leaving the final set of rows to be returned from the query.

Conclusion

Not every query needs to have all the parts we listed above, but a part of why SQL is so flexible is that it allows developers and data analysts to quickly manipulate data without having to write additional code, all just by using the above clauses.

<br/><br/>


| Data type                                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `INTEGER`, `BOOLEAN`                                 | The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1.                                                                                                                                                                                                                                                    |
| `FLOAT`, `DOUBLE`, `REAL`                            | The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value.                                                                                                                                                                                                                                        |
| `CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT` | The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.

<br/>

Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables. |
<br/>

| `DATE`, `DATETIME`                                   | SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones.                                                                                                                                                                                                                                                                         |
| `BLOB`                                               | Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them.                                                                                                                                                                                                                                                     |


<br/><br/>
| Constraint           | Description                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `PRIMARY KEY`        | This means that the values in this column are unique, and each value can be used to identify a single row in this table.                                                                                                                                                                                                                                                                         |
| `AUTOINCREMENT`      | For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases.                                                                                                                                                                                                                                                |
| `UNIQUE`             | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the \`PRIMARY KEY\` in that it doesn't have to be a key for a row in the table.                                                                                                                                        |
| `NOT NULL`           | This means that the inserted value can not be \`NULL\`.                                                                                                                                                                                                                                                                                                                                          |
| `CHECK (expression)` | This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc.                                                                                                                                                                       |
| `FOREIGN KEY`        | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.<br><br>For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the \`FOREIGN KEY\` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list. |
