# SQL, DBMS, Python, and Markdown -- Questions and Answers

## 1. What is the order of execution in SQL?

SQL has a logical order of execution. A common order for a `SELECT`
query is:

1.  `FROM` and `JOIN` -- identify the tables and combine rows.
2.  `WHERE` -- filter individual rows.
3.  `GROUP BY` -- group the remaining rows.
4.  `HAVING` -- filter the groups.
5.  `SELECT` -- choose the columns and calculate expressions.
6.  `DISTINCT` -- remove duplicate result rows.
7.  `ORDER BY` -- sort the result.
8.  `LIMIT` / `OFFSET` -- return a specific number of rows and skip
    rows.


## 2. What are the different constraints used in SQL?

Constraints are rules applied to table columns to maintain valid and
consistent data.

-   **NOT NULL** -- prevents a column from storing `NULL`.
-   **UNIQUE** -- ensures that values in a column or combination of
    columns are unique.
-   **PRIMARY KEY** -- uniquely identifies each row. It combines the
    properties of `UNIQUE` and `NOT NULL`.
-   **FOREIGN KEY** -- ensures that a value refers to an existing key in
    another table (or an eligible key in the same table).
-   **CHECK** -- ensures that values satisfy a specified condition.
-   **DEFAULT** -- supplies a value when an insert does not provide one
    for that column.


## 3. Can a table have more than one primary key?

No. A table can have only **one primary key constraint**. However, that
primary key can contain more than one column. This is called a
**composite primary key**.

A table can also have multiple `UNIQUE` constraints.


Here, the combination of `student_id` and `course_id` uniquely
identifies each row.

## 4. What is OFFSET in SQL?

`OFFSET` skips a specified number of rows before returning the result.
It is commonly used for pagination.

**Example:**

``` sql
LIMIT 10 OFFSET 20;
```

This skips the first 20 rows and returns the next 10 rows. Use
`ORDER BY` to make pagination predictable.

In PostgreSQL, `LIMIT` and `OFFSET` are commonly used together. MySQL
also supports this syntax.

## 5. What is the use of DEFAULT in SQL?

`DEFAULT` assigns a predefined value to a column when an `INSERT`
statement does not provide a value for that column.


## 6. Which is faster: a tuple or a list in Python?

Tuples are generally slightly faster and use less memory than lists for
similar collections of items because tuples are immutable. However, the
difference is usually small.

-   **List (`[]`)** -- mutable; items can be added, removed, or changed.
-   **Tuple (`()`)** -- immutable; its elements cannot be reassigned
    after creation.

Choose a list when the collection needs to change. Choose a tuple when
the collection should remain fixed. Prefer the data structure that fits
the task rather than choosing only for speed.

## 7. What are some common formats in Markdown?

Markdown is a lightweight markup language used to format documents,
notes, and README files.

Common formats include:

**Headings**

``` markdown
# Heading 1
## Heading 2
### Heading 3
```

**Bold and italic**

``` markdown
**Bold text**
*Italic text*
~~Strikethrough text~~
```


## 8. What is a composite key?

A composite key is a key made up of two or more columns. The combination
of those columns uniquely identifies a row.

**Example:**

``` sql
CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    enrolled_on DATE,
    PRIMARY KEY (student_id, course_id)
);
```

A student can enrol in many courses, and a course can have many
students. The combination of `student_id` and `course_id` prevents the
same student-course pair from being stored more than once.

## 9. Can we use aggregate functions without GROUP BY?

Yes. Aggregate functions can be used without `GROUP BY`. In that case,
the function calculates a result over all rows selected by the query and
returns one result row.

**Example:**

``` sql
SELECT COUNT(*) AS total_employees,
       AVG(salary) AS average_salary
FROM employees;
```

This returns the total number of employees and their average salary.

If a `WHERE` clause is present, the aggregate is calculated only over
rows that pass that filter. Without `GROUP BY`, do not select ordinary
columns alongside aggregates unless the database allows them through a
specific rule or they are themselves aggregated.

## 10. Can we use HAVING before GROUP BY?

No. In the written syntax of a `SELECT` query, `GROUP BY` comes before
`HAVING`.

-   `WHERE` filters individual rows before grouping.
-   `HAVING` filters groups after grouping.

**Example:**

``` sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;
```

This returns only departments that have more than five employees.

## 11. What is the order of writing commands in SQL?

The usual written order of clauses in a `SELECT` statement is:

``` sql
SELECT column_names
FROM table_name
JOIN another_table ON join_condition
WHERE row_condition
GROUP BY column_names
HAVING group_condition
ORDER BY column_names
LIMIT number_of_rows
OFFSET number_of_rows;
```

Not every query needs every clause. For example, a simple query may use
only `SELECT` and `FROM`.

Remember: the order in which clauses are written is different from the
logical order in which SQL processes them.

## 12. What are the disadvantages of indexing?

Indexes improve data retrieval in many cases, but they also have
disadvantages:

1.  **Extra storage** -- indexes require additional disk space.
2.  **Slower inserts** -- the database must update relevant indexes when
    new rows are inserted.
3.  **Slower updates and deletes** -- changes to indexed values or rows
    may require index maintenance.
4.  **Maintenance overhead** -- indexes may need maintenance and
    monitoring.
5.  **Poor index choices** -- unnecessary or unsuitable indexes can use
    resources without improving query performance.

Indexes should be created on columns that are frequently used in
searches, joins, and sorting when they provide a measurable benefit.
More indexes do not always mean better performance.

## 13. What is the difference between CHAR and VARCHAR?

Both `CHAR` and `VARCHAR` store character strings, but they handle
length differently.

  -----------------------------------------------------------------------
  Feature                 CHAR(n)                 VARCHAR(n)
  ----------------------- ----------------------- -----------------------
  Length                  Fixed-length type       Variable-length type

  Storage behaviour       Values are padded to the declared length    Stores the value without fixed-width
                            

  Best suited for         Values that have a values whose lengths
                          consistent length       vary

 

If a value such as `'IN'` is stored in `CHAR(2)`, it fits the declared
length. `VARCHAR(100)` can store names of different lengths.


## 14. What is DDL in SQL, and do we use it?

DDL stands for **Data Definition Language**. It is used to define and
modify database structures such as tables, schemas, and indexes.

Common DDL commands include:

-   `CREATE` -- creates database objects.
-   `ALTER` -- changes the structure of an existing object.
-   `DROP` -- removes a database object.
-   `TRUNCATE` -- removes all rows from a table while keeping the table
    structure.

DDL is used regularly when creating and managing database
structures.

## 15. What is the use of the underscore (\_) while matching in SQL?

In SQL's `LIKE` pattern, the underscore (`_`) matches **exactly one
character**.

The percent sign (`%`) matches zero or more characters.

For example, `_am` matches `Ram` and `Sam`, but not `AamK`.

## 16. How is data stored in an RDBMS?

An RDBMS (Relational Database Management System) stores data in tables
made up of rows and columns.

-   **Table** -- stores data about an entity, such as employees or
    departments.
-   **Row** -- represents one record.
-   **Column** -- represents an attribute, such as employee name or
    salary.
-   **Primary key** -- uniquely identifies a row.
-   **Foreign key** -- connects related tables.


## 17. What are the different types of DBMS?

DBMS types are commonly classified according to how they organize and
manage data.

1.  **Hierarchical DBMS** -- organizes data in a tree-like parent-child
    structure. A child record generally has one parent.
2.  **Network DBMS** -- organizes data as a network of records, allowing
    a record to have multiple relationships.
3.  **Relational DBMS (RDBMS)** -- stores data in tables with rows and
    columns. Tables can be related using keys. Examples: PostgreSQL,
    MySQL, and Oracle Database.
4.  **Object-Oriented DBMS (OODBMS)** -- stores data as objects, using
    concepts such as classes and object identity.
5.  **NoSQL DBMS** -- supports data models beyond the traditional
    relational table model. Common categories include document,
    key-value, wide-column, and graph databases. Examples include
    MongoDB, Redis, Cassandra, and Neo4j.

The best type depends on the application's data structure,
relationships, consistency requirements, and workload.
