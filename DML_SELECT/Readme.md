## SQL SELECT Statement

> The SELECT statement is an SQL command used to retrieve data from one or more tables in a database.

## SQL SELECT Statement Structure
```sql
SELECT [DISTINCT] column_list , scalar_expressions
FROM table_name / joined_tables / subquery
WHERE filtering_condition
GROUP BY column_list
HAVING aggregation_filtering_condition
ORDER BY column_list / scalar_expressions [ASC | DESC]
LIMIT {x};
```
```SQL
SELECT title , release_year
FROM film
ORDER BY 2 , 1;
```
```SQL
SELECT title AS movie_title , release_year AS movie_release_year
FROM film
ORDER BY movie_release_year;
```

**Unique Column**
```SQL
SELECT DISTINCT rating
FROM film
ORDER BY rating;
```
```SQL
SELECT rating
FROM film
GROUP BY rating
ORDER BY rating;
```
**Concat String**
```SQL
SELECT first_name || ' ' || last_name AS full_name
FROM actor
ORDER BY first_name , last_name;
```
**Filtering**
```SQL
SELECT first_name || ' ' || last_name AS full_name
FROM actor
WHERE last_name = 'cruise' -- Case sensitive
ORDER BY first_name , last_name;
```

**Filtering**
```SQL
SELECT first_name || ' ' || last_name AS full_name
FROM actor
WHERE last_name = 'cruise' -- Case sensitive
ORDER BY first_name , last_name;
```
**Matching Pattern**
```SQL
SELECT first_name || ' ' || last_name AS full_name
FROM actor
WHERE last_name LIKE 'BAR%' -- '%' : 0 or many character , _ : only one character 
ORDER BY first_name , last_name;
```
**Several Option in one query**
```SQL
SELECT first_name || ' ' || last_name AS full_name
FROM actor
WHERE last_name IN ('cruise' , 'barrymore' , 'hopkins') -- Case sensitive
ORDER BY first_name , last_name;
```
**Logical operation**
```SQL
SELECT 2 + 2*2 = 6 ; -- TRUE OR FALSE
```
**Substring of string**
```SQL
SELECT SUBSTRING('Leonardo' ,1 , 3); -- string,start , end(optional)
```
**Check word in string**
```SQL
SELECT POSITION('0' ,'Leonardo'); -- first occurence
```
**Greatest in List**
```SQL
SELECT GREATEST(4 , 8 , 19.99 , -24 , null)
```
**Smallest in List**
```SQL
SELECT LEAST(4 , 8 , 19.99 , -24 , null)
```
**Determine the data type of an expression or value**.
```sql
SELECT pg_typeof(expression);
--- SELECT PG_TYPEOF(LEAST(4,-8 ,19.99 ,-24 ,null)
```
**COALESCE Function**
```SQL
SELECT COALESCE(NULL, NULL, 'Hello', 'World');  -- Returns 'Hello'
```
**Explicit type conversion**
```SQL
SELECT rating::TEXT
FROM film
```

## JOIN Types
## Inner Join ( A ∩ B )

Table A                                                     
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

Table B
| ID | Name |
|----|------|
| 1  | LMN  |
| 4  | OPQ  |
| 4  | RST  |


### SQL Query

```sql
SELECT * 
FROM A 
INNER JOIN B 
ON A.ID = B.ID;
```
Table A ∩ B
| A .ID |A .Name| B .ID |  B .Name
|----  |------|-----|----
| 1    | X    |1 | LMN


## Left Outer Join ( A ∪ (A ∩ B) )


**Table A:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

**Table B:**
| ID | Name |
|----|------|
| 1  | LMN  |
| 4  | OPQ  |
| 4  | RST  |

### SQL Query
```sql
SELECT * FROM
A LEFT [OUTER] JOIN B
ON A.ID = B.ID
```

### Result

| A .ID | A .Name | B .ID | B .Name |
|------|--------|------|--------|
| 1    | X      | 1    | LMN    |
| 2    | Y      | null | null   |
| 3    | Z      | null | null   |

## Explanation

A LEFT OUTER JOIN returns all records from the left table (A), and the matched records from the right table (B). If there is no match, NULL values are returned for columns from the right table.

In this example:
- Row with ID=1 exists in both tables, so it returns matched data
- Rows with ID=2 and ID=3 only exist in Table A, so B .ID and B .Name show null
- Rows with ID=4 from Table B are not included since they don't match any rows in Table A

## Right Outer Join ( B ∪ (A ∩ B) )

**Table A:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

**Table B:**
| ID | Name |
|----|------|
| 1  | LMN  |
| 4  | OPQ  |
| 4  | RST  |

### SQL Query
```sql
SELECT * FROM
A RIGHT [OUTER] JOIN B
ON A.ID = B.ID
```

### Result

| A .ID | A .Name | B .ID | B .Name |
|------|--------|------|--------|
| 1    | X      | 1    | LMN    |
| null | null   | 4    | OPQ    |
| null | null   | 4    | RST    |

## Explanation

A RIGHT OUTER JOIN returns all records from the right table (B), and the matched records from the left table (A). If there is no match, NULL values are returned for columns from the left table.

In this example:
- Row with ID=1 exists in both tables, so it returns matched data
- Rows with ID=4 only exist in Table B, so A .ID and A .Name show null (two rows because there are two records with ID=4 in Table B)
- Rows with ID=2 and ID=3 from Table A are not included since they don't match any rows in Table B

##  Full Outer Join ( A ∪ B )


**Table A:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

**Table B:**
| ID | Name |
|----|------|
| 1  | LMN  |
| 4  | OPQ  |
| 4  | RST  |

### SQL Query
```sql
SELECT * FROM
A FULL [OUTER] JOIN B
ON A.ID = B.ID
```

### Result

| A .ID | A .Name | B .ID | B .Name |
|------|--------|------|--------|
| 1    | X      | 1    | LMN    |
| 2    | Y      | null | null   |
| 3    | Z      | null | null   |
| null | null   | 4    | OPQ    |
| null | null   | 4    | RST    |

## Explanation

A FULL OUTER JOIN returns all records when there is a match in either the left table (A) or the right table (B). It combines the results of both LEFT and RIGHT OUTER JOINs.

In this example:
- Row with ID=1 exists in both tables, so it returns matched data
- Rows with ID=2 and ID=3 only exist in Table A, so B .ID and B .Name show null
- Rows with ID=4 only exist in Table B, so A .ID and A .Name show null (two rows because there are two records with ID=4 in Table B)
- All records from both tables are included in the result

## Cross Join

**Table A:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

**Table B:**
| ID | Name |
|----|------|
| 1  | LMN  |
| 4  | OPQ  |
| 4  | RST  |

### SQL Query
```sql
SELECT * FROM
A CROSS JOIN B
```

### Result

| A .ID | A .Name | B .ID | B .Name |
|------|--------|------|--------|
| 1    | X      | 1    | LMN    |
| 1    | X      | 4    | OPQ    |
| 1    | X      | 4    | RST    |
| 2    | Y      | 1    | LMN    |
| 2    | Y      | 4    | OPQ    |
| 2    | Y      | 4    | RST    |
| 3    | Z      | 1    | LMN    |
| 3    | Z      | 4    | OPQ    |
| 3    | Z      | 4    | RST    |

## Explanation

A CROSS JOIN returns the Cartesian product of both tables, meaning every row from Table A is combined with every row from Table B. No join condition is specified.

In this example:
- Table A has 3 rows and Table B has 3 rows
- The result has 3 × 3 = 9 rows
- Each row from Table A appears with every row from Table B

---

## Bad Practice: Using WHERE for CROSS JOIN

#### ⚠️ CROSS JOIN + WHERE A .ID = B .ID

### SQL Query (Bad Practice)
```sql
SELECT * FROM
A CROSS JOIN B
WHERE A.ID = B.ID
```

### Result

| A .ID | A .Name | B .ID | B .Name |
|------|--------|------|--------|
| 1    | X      | 1    | LMN    |

## Why This Is Bad Practice

While using `CROSS JOIN` with a `WHERE` clause to filter results technically works, it is **not recommended** because:

1. **Performance**: Creates unnecessary Cartesian product first (all combinations), then filters
2. **Clarity**: Makes the intent unclear - you're actually doing an INNER JOIN
3. **Best Practice**: Use explicit `INNER JOIN` with `ON` clause instead

### Better Alternative
```sql
SELECT * FROM
A INNER JOIN B
ON A.ID = B.ID
```

This achieves the same result more efficiently and makes the intent clear.


# Aggregation Queries

### Basic Syntax
```sql
SELECT AGGREGATION_FUNCTION(column)
FROM table;
```

## Common Aggregation Functions

- **SUM** - Calculates the total sum of a numeric column
- **AVG** - Calculates the average value of a numeric column
- **MIN** - Finds the minimum value in a column
- **MAX** - Finds the maximum value in a column
- **COUNT** - Counts the number of rows

---

## Examples with Table_A

### Sample Data

**Table_A:**
| ID | Amount |
|----|--------|
| 1  | 10     |
| 2  | 20     |
| 3  | null   |
| 4  | 40     |

### COUNT Examples

#### COUNT(*) - Counts all rows
```sql
SELECT COUNT(*)
FROM Table_A;
```

**Result:**
| count |
|-------|
| 4     |

#### COUNT(column) - Counts non-null values
```sql
SELECT COUNT(Amount)
FROM Table_A;
```

**Result:**
| count |
|-------|
| 3     |

**Note:** `COUNT(Amount)` only counts rows where Amount is NOT NULL.

### Multiple Aggregations
```sql
SELECT SUM(Amount),
       AVG(Amount),
       MIN(Amount),
       MAX(Amount)
FROM Table_A;
```

**Result:**
| sum | avg  | min | max |
|-----|------|-----|-----|
| 70  | 23.3 | 10  | 40  |

---

## Handling NULL Values

### When All Values Are NULL

**Table_A (all nulls):**
| ID | Amount |
|----|--------|
| 1  | null   |
| 2  | null   |
| 3  | null   |
| 4  | null   |
```sql
SELECT COUNT(*)
FROM Table_A;
```

**Result:**
| count |
|-------|
| 4     |
```sql
SELECT COUNT(Amount)
FROM Table_A;
```

**Result:**
| count |
|-------|
| 0     |
```sql
SELECT SUM(Amount),
       AVG(Amount),
       MIN(Amount),
       MAX(Amount)
FROM Table_A;
```

**Result:**
| sum  | avg  | min  | max  |
|------|------|------|------|
| null | null | null | null |

**Important:** Aggregation functions (except COUNT) return NULL when operating on columns with all NULL values.

---

## GROUP BY

### Basic Syntax
```sql
SELECT column_1,
       AGGREGATION_FUNCTION(column_2)
FROM table
GROUP BY column_1;
```

## Example: Film Ratings

### Without GROUP BY
```sql
SELECT film_id,
       rating
FROM film
WHERE film_id < 8
ORDER BY rating;
```

**Result:**
| film_id | rating |
|---------|--------|
| 4       | G      |
| 2       | G      |
| 5       | G      |
| 1       | PG     |
| 6       | PG     |
| 7       | PG-13  |
| 3       | NC-17  |

### With GROUP BY and COUNT
```sql
SELECT rating,
       COUNT(*) AS film  -- counts number of rows
FROM film
WHERE film_id < 8
GROUP BY rating
ORDER BY rating;
```

**Result:**
| rating | films |
|--------|-------|
| G      | 3     |
| PG     | 2     |
| PG-13  | 1     |
| NC-17  | 1     |

---

## WHERE vs HAVING

### Using WHERE (filters before grouping)
```sql
SELECT rating,
       COUNT(*) AS film
FROM film
WHERE film_id < 8 AND COUNT(*) > 1  -- ❌ WRONG! Can't use aggregation in WHERE
GROUP BY rating
ORDER BY rating;
```

**This is INCORRECT!** You cannot use aggregation functions in the WHERE clause.

### Using HAVING (filters after grouping)
```sql
SELECT rating,
       COUNT(*) AS film
FROM film
WHERE film_id < 8
GROUP BY rating
HAVING COUNT(*) > 1  -- ✅ CORRECT! Filter groups after aggregation
ORDER BY rating;
```

**Result:**
| rating | films |
|--------|-------|
| G      | 3     |
| PG     | 2     |

---

## HAVING Clause

### Purpose

The `HAVING` clause filters grouped results based on aggregate conditions.

## Syntax
```sql
SELECT column_1,
       AGGREGATION_FUNCTION(column_2)
FROM table
GROUP BY column_1
HAVING AGGREGATION_FUNCTION(column_2) > X;
```

## Complete Example
```sql
SELECT rating,
       COUNT(*) AS film
FROM film
WHERE film_id < 8
GROUP BY rating
HAVING COUNT(*) > 1
ORDER BY rating;
```

**Result:**
| rating | films |
|--------|-------|
| G      | 3     |
| PG     | 2     |

---

### Key Differences: WHERE vs HAVING

| Aspect | WHERE | HAVING |
|--------|-------|--------|
| **Filters** | Individual rows | Grouped results |
| **Applied** | Before GROUP BY | After GROUP BY |
| **Can use aggregations** | ❌ No | ✅ Yes |
| **Example** | `WHERE film_id < 8` | `HAVING COUNT(*) > 1` |

### Execution Order

1. **FROM** - Get data from table
2. **JOIN** - Combine tables based on join conditions
3. **WHERE** - Filter individual rows
4. **GROUP BY** - Group the filtered rows
5. **HAVING** - Filter the grouped results
6. **SELECT** - Select columns to display
7. **ORDER BY** - Sort the final results
8. **LIMIT** - Restrict output

## SQL Subqueries

A subquery is a query nested inside another query. The subquery executes first, and its result is used by the outer (main) query.

### Basic Structure
```sql
SELECT *
FROM table_A
WHERE column_1 [OPERATOR]
    (SELECT column_3
     FROM table_B
     WHERE ...);
```

### Common Operators Used with Subqueries

- `=` - Equal to
- `IN` - Value exists in subquery results
- `NOT IN` - Value does not exist in subquery results
- `>` - Greater than
- `>=` - Greater than or equal to
- `<` - Less than
- `<=` - Less than or equal to
- `!=` - Not equal to
- `ALL` - Compare value to all values returned by subquery
- `ANY` - Compare value to any value returned by subquery
- `EXISTS` - Returns TRUE if subquery returns any rows
- `NOT EXISTS` - Returns TRUE if subquery returns no rows

### Example 1: Subquery in FROM Clause
```sql
SELECT max_len_film.title AS movie_title,
       a.first_name || ' ' || a.last_name AS actor_full_name
FROM 
    (SELECT *
     FROM film f
     ORDER BY f.length DESC
     LIMIT 3) AS max_len_film
INNER JOIN film_actor fa
    ON fa.film_id = max_len_film.film_id
INNER JOIN actor a
    ON a.actor_id = fa.actor_id;
```

**Purpose**: Find actors in the 3 longest films.

### Example 2: IN Operator with Subquery
```sql
SELECT *
FROM A
WHERE A.ID IN
    (SELECT B.ID
     FROM B
     WHERE B.Name = 'LMN');
```

**How it works**:
1. Subquery finds all IDs from table B where Name = 'LMN'
2. Main query returns all rows from A where A.ID matches those IDs
3. Result: Returns rows with ID 1 and 2 from table A

### Example 3: EXISTS Operator
```sql
SELECT *
FROM A
WHERE EXISTS
    (SELECT *
     FROM B
     WHERE B.Name = 'LMN'
     AND A.ID = B.ID);
```

**How EXISTS works**:
- Returns TRUE if the subquery returns one or more rows
- For each row in table A, checks if a matching row exists in table B
- The subquery correlates with the outer query through `A.ID = B.ID`
- Result: Returns rows from A that have matching records in B where Name = 'LMN'

**Key difference from IN**: EXISTS only checks for existence (returns boolean), while IN compares actual values.

### Example 4: Nested Subqueries (Multiple Levels)

 **Finding films by a specific actor (Burt Temple)**
```sql
-- Method 1: Using nested subqueries with equals operator
SELECT fa.film_id
FROM film_actor fa
WHERE fa.actor_id = 
    (SELECT a.actor_id
     FROM actor a
     WHERE UPPER(a.first_name) = 'BURT' 
     AND UPPER(a.last_name) = 'TEMPLE');
```

**Explanation**: 
- Innermost query finds the actor_id for "Burt Temple"
- Outer query finds all film_ids associated with that actor_id

### Finding film details with nested subqueries
```sql
-- Method 2: Using IN with nested subqueries
SELECT f.title,
       f.release_year
FROM film f
WHERE f.film_id IN (
    SELECT fa.film_id
    FROM film_actor fa
    WHERE fa.actor_id = 
        (SELECT a.actor_id
         FROM actor a
         WHERE UPPER(a.first_name) = 'BURT' 
         AND UPPER(a.last_name) = 'TEMPLE')
);
```

**How nested subqueries execute**:
1. **Innermost subquery** executes first: Gets actor_id for "Burt Temple"
2. **Middle subquery** executes next: Gets all film_ids for that actor
3. **Outer query** executes last: Gets title and release_year for those films

**Results** (sample):
- Brannigan Sunrise (2004)
- Adaptation Holes (2003)
- Chamber Italian (1996)
- Grosse Wonderful (1999)
- Airport Pollock (2000)
- And more...

### Using EXISTS with nested subqueries
```sql
-- Method 3: Using EXISTS with correlated nested subqueries
SELECT f.title,
       f.release_year
FROM film f
WHERE EXISTS (
    SELECT fa.film_id
    FROM film_actor fa
    WHERE fa.actor_id = 
        (SELECT a.actor_id
         FROM actor a
         WHERE UPPER(a.first_name) = 'BURT' 
         AND UPPER(a.last_name) = 'TEMPLE')
    AND f.film_id = fa.film_id
);
```

**Key aspects**:
- The EXISTS clause checks if there's at least one matching record
- The subquery is correlated with the outer query via `f.film_id = fa.film_id`
- This correlates the film table with the results of the nested subquery

### Example 5: NOT IN Operator
```sql
SELECT 2 NOT IN (1, 3, 5, 8, NULL);
```

**Result**: Returns `NULL` (not TRUE or FALSE)

**Important NOT IN behavior with NULL**:
- When the list contains NULL, `NOT IN` returns NULL instead of TRUE or FALSE
- This is because SQL cannot definitively say whether a value is "not in" a list that contains an unknown (NULL) value
- This can cause unexpected behavior in WHERE clauses, as rows where the condition evaluates to NULL are filtered out

**Practical example**:
```sql
SELECT column_name
FROM table_name
WHERE column_name NOT IN (1, 3, 5, 8, NULL);
```
This query returns NO rows because the condition evaluates to NULL for all values.

**Safe alternative when NULLs might exist**:
```sql
SELECT column_name
FROM table_name
WHERE column_name NOT IN (
    SELECT value 
    FROM other_table 
    WHERE value IS NOT NULL
);
```

### Example 6: NOT EXISTS Operator
```sql
-- Find films that Burt Temple did NOT appear in
SELECT f.title,
       f.release_year
FROM film f
WHERE NOT EXISTS (
    SELECT fa.film_id
    FROM film_actor fa
    WHERE fa.actor_id = 
        (SELECT a.actor_id
         FROM actor a
         WHERE UPPER(a.first_name) = 'BURT' 
         AND UPPER(a.last_name) = 'TEMPLE')
    AND f.film_id = fa.film_id
);
```

**How NOT EXISTS works**:
- Returns TRUE if the subquery returns NO rows
- Returns FALSE if the subquery returns any rows
- Unlike NOT IN, NOT EXISTS handles NULL values correctly and safely
- It's often preferred over NOT IN for better NULL handling and performance

**Key differences between NOT IN and NOT EXISTS**:

| Feature | NOT IN | NOT EXISTS |
|---------|--------|------------|
| NULL Handling | Returns NULL if list contains NULL | Handles NULLs safely |
| Performance | Can be slower on large datasets | Generally faster with proper indexing |
| Correlation | Cannot be correlated | Can be correlated with outer query |
| Best Use Case | Small, static lists without NULLs | Checking non-existence in related tables |

### Subquery Nesting Levels
---
Subqueries can be nested multiple levels deep:
- **Level 1**: Simple subquery (one level of nesting)
- **Level 2**: Nested subquery (subquery within a subquery)
- **Level 3+**: Multiple levels of nesting (generally should be avoided for readability)

**Important Note**: While SQL allows deep nesting, queries with more than 2-3 levels become difficult to read and maintain. Consider using CTEs (Common Table Expressions) or temporary tables for complex queries.

## Best Practices

1. **Performance**: Subqueries in the WHERE clause are evaluated for each row, which can be slow for large datasets
2. **Readability**: Use meaningful aliases for subqueries in FROM clauses
3. **Alternative**: Consider using JOINs when possible for better performance
4. **Correlated vs Non-correlated**: 
   - Non-correlated subqueries execute once
   - Correlated subqueries (like with EXISTS) execute for each row in the outer query
5. **String Comparison**: Use UPPER() or LOWER() functions for case-insensitive comparisons
6. **Nesting Depth**: Limit nesting to 2-3 levels maximum for maintainability
7. **NULL Handling**: 
   - Prefer NOT EXISTS over NOT IN when checking for non-existence
   - Always consider NULL values when using IN or NOT IN
   - Filter out NULLs explicitly in subqueries used with NOT IN
8. **Testing**: Always test queries with edge cases including NULL values

## Common Use Cases

- Finding records that match a filtered set of values
- Calculating aggregates to use in comparisons
- Checking for the existence of related records
- Finding records that DON'T have related records (using NOT EXISTS)
- Creating derived tables for complex queries
- Multi-level filtering across related tables
- Finding records based on conditions in distant related tables
- Excluding records based on conditions in other tables

## Performance Considerations

When working with nested subqueries:
- The innermost query executes first, then works outward
- Each level adds processing overhead
- Consider query execution plans to optimize performance
- JOINs often perform better than nested subqueries
- Use indexes on columns referenced in subquery WHERE clauses
- NOT EXISTS typically performs better than NOT IN for large datasets
- Consider using CTEs (WITH clause) for complex queries to improve readability and potentially performance

> ### NULL Values Warning
> **CRITICAL**: Be extremely careful with NOT IN when NULL values might be present in the subquery results. This is one of the most common sources of unexpected query behavior in SQL. When in doubt, use NOT EXISTS instead.


# SQL Advanced Concepts 

## Common Table Expressions (CTE)

### Key Points about CTEs

1. **Improves readability** - No need to read the query upside down
2. **Allows reusing the same subquery** multiple times without duplicating the code
3. **In PostgreSQL**, CTEs can be thought of 'materialized' subqueries
4. **CTEs are defined using the WITH clause**
5. **For simple single-use subqueries CTEs are not needed**

### CTE Syntax
```sql
WITH cte_name_X AS (
    SELECT B.column_3
    FROM table_B AS B
    JOIN table_C AS C
    ON B.column_4 = C.column_5
    WHERE ...
)
SELECT column_1, column_2
FROM table_A
WHERE column_1 IN (
    SELECT column_3
    FROM cte_name_X
);
```

### Example: Finding Films by Actor
```sql
-- Find all films featuring actor "Burt Temple"
WITH film_burt_temple AS (
    SELECT fa.film_id
    FROM film_actor fa
    WHERE fa.actor_id = (
        SELECT a.actor_id
        FROM actor a
        WHERE UPPER(a.first_name) = 'BURT' 
          AND UPPER(a.last_name) = 'TEMPLE'
    )
)
SELECT f.title,
       f.release_year
FROM film f
WHERE f.film_id IN (
    SELECT film_id
    FROM film_burt_temple
);
```

**Benefits of this CTE approach:**
- The subquery `film_burt_temple` is named and easier to understand
- Can be reused multiple times if needed
- Separates the logic into clear, manageable steps

---

## Comparison: CTE vs View vs Subquery

| Feature | CTE | View | Subquery |
|---------|-----|------|----------|
| **Purpose** | Temporary query result | Reusable query result | Part of a larger query |
| **Usage** | Defined in a single SQL statement | Defined as a separate database object | Nested within a larger query |
| **Syntax** | Defined using the keyword WITH | Defined using the CREATE VIEW statement | Defined within parentheses |
| **Naming** | Named within the query | Named as a separate database object | Not named |
| **Performance** | Generally less efficient due to lack of optimization | Generally more efficient, as they can be indexed | Generally less efficient due to repeated calculations |
| **Data Persistence** | Not persisted in the database | Persisted in the database | Not persisted in the database |
| **Functionality** | Can be recursive | Can be indexed | Can only return scalar or aggregate values |
| **Readability** | Can improve the readability of complex queries | Can improve the readability of complex queries | Can make queries more complex and harder to read |

---

## Set Operations:

### Prerequisites for Set Operations

**Identical output structure:**
- Same number of columns
- Corresponding columns have compatible datatypes

### Set Operations Comparison Table

| Set Operation | Description | Handles Duplicates | Use Case |
|---------------|-------------|-------------------|----------|
| **UNION ALL** | Combines all rows from both queries | Keeps all duplicates | When you need all records and duplicates matter, faster performance |
| **UNION** | Combines rows from both queries | Removes duplicates | When you need unique records only |
| **INTERSECT** | Returns only common rows | Returns unique common rows | Finding records that exist in both datasets |
| **EXCEPT** | Returns rows from first query not in second | Returns unique differences | Finding records in one dataset but not another |

### Basic Structure
```sql
SELECT column_1, column_2
FROM table_A

{SET OPERATION}

SELECT column_1, column_2
FROM table_B
```

---

### UNION ALL

Combines all rows from both tables, **including duplicates**.

**Example:**
```sql
SELECT *
FROM A

UNION ALL

SELECT *
FROM B
```

**Input Tables:**

Table A:
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |

Table B:
| ID | Name |
|----|------|
| 1  | X    |
| 4  | Q    |

**Result:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |
| 1  | X    |
| 4  | Q    |

**Note:** Same data type in columns, same number of columns. Duplicates are retained.

---

### UNION

Combines rows from both tables, **removing duplicates**.

**Example:**
```sql
SELECT *
FROM A

UNION

SELECT *
FROM B
```

**Result:**
| ID | Name |
|----|------|
| 1  | X    |
| 2  | Y    |
| 3  | Z    |
| 4  | Q    |

---

### INTERSECT

Returns only rows that exist in **both queries**.

**Example:**
```sql
SELECT *
FROM A

INTERSECT

SELECT *
FROM B
```

**Result:**
| ID | Name |
|----|------|
| 1  | X    |

---

### EXCEPT

Returns rows from the first query that **don't appear** in the second query.

**Example:**
```sql
SELECT *
FROM A

EXCEPT

SELECT *
FROM B
```

**Result:**
| ID | Name |
|----|------|
| 2  | Y    |
| 3  | Z    |

---

## Practical Example: Complex Subquery with HAVING Clause

### Problem Statement

Find the **top 3 longest films** (title, release year, length) from the last 10 years that are:
- Rated PG-13 or higher
- Available in at least two DVD rental shops

### Solution
```sql
-- Top 3 longest films from last 10 years
-- Available in at least two DVD rental shops
SELECT f.title,
       f.release_year,
       f.length
FROM film f
WHERE f.release_year >= EXTRACT(year FROM current_date) - 10
  AND f.rating IN ('PG-13', 'R', 'NC-17')
  AND f.film_id IN (
      SELECT inv.film_id
      FROM inventory inv
      GROUP BY inv.film_id
      HAVING COUNT(DISTINCT inv.store_id) >= 2
  )
ORDER BY f.length DESC
LIMIT 3;
```

### Query Breakdown

#### Main Query:
- Selects `title`, `release_year`, and `length` from the film table
- Filters for films from the last 10 years using `EXTRACT(year FROM current_date) - 10`
- Filters for appropriate ratings: PG-13, R, or NC-17

#### Subquery (IN clause):
- Queries the `inventory` table to find films available in multiple stores
- Groups results by `film_id`
- Uses `HAVING` clause to filter only films available in 2 or more distinct stores
- The `COUNT(DISTINCT inv.store_id) >= 2` ensures availability in at least two locations

#### Result Ordering:
- `ORDER BY f.length DESC` sorts by film length in descending order
- `LIMIT 3` returns only the top 3 longest films

### Key Concepts Demonstrated

1. **Subquery with Aggregation** - Using COUNT and GROUP BY within a subquery
2. **HAVING Clause** - Filtering aggregated results (stores >= 2)
3. **DISTINCT Keyword** - Counting unique store locations
4. **Date Functions** - Using EXTRACT to calculate date ranges dynamically
5. **IN Operator** - Filtering based on subquery results



## Best Practices

### When to Use Each Approach

- **Use CTEs** for complex queries that need temporary named results within a single statement
- **Use Views** when you need to reuse query logic across multiple queries or applications
- **Use Subqueries** for simple, one-time filtering or calculation needs
- **Use HAVING clause** when you need to filter on aggregated data (after GROUP BY)

### Set Operations Guidelines

- Choose **UNION ALL** when you need duplicates and want better performance
- Choose **UNION** when you need unique results only
- Remember that **INTERSECT** and **EXCEPT** can be useful for data comparison and validation tasks

### Performance Tips

- Use **DISTINCT with COUNT** when you need to count unique values (like distinct stores)
- Prefer **dynamic date calculations** (like EXTRACT) over hardcoded dates for maintainable queries
- Consider creating **indexes on Views** for frequently accessed query patterns
- **CTEs** can improve readability but may not always be optimized - test performance for critical queries

### Code Quality

- Always use clear, descriptive names for CTEs and Views
- Add comments to explain complex logic
- Break down complex queries into smaller, understandable parts
- Test queries with small datasets first before running on production data 
