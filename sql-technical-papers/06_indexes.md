# Technical Paper 06: Indexes in DBMS

**Subject:** Database Management Systems  
**Topic:** Database Indexes  
**Focus:** Improving Data Retrieval Performance

---

## 1. Introduction to Indexes

An index is a database structure that helps the database find rows more quickly.

Without an index, the database may need to examine many rows to find the records that match a query. This is often called a **table scan** or **sequential scan**.

An index works like the index at the back of a book. Instead of reading every page, we use the index to find where the required information is located.

### Example

Suppose an `employees` table contains one million records.

```sql
SELECT *
FROM employees
WHERE employee_id = 50000;
```

If there is no suitable index, the database may need to examine many rows.

An index on `employee_id` can help the database locate the matching row more efficiently.

## 2. Creating an Index

A basic index can be created using `CREATE INDEX`.

```sql
CREATE INDEX idx_employee_name
ON employees (employee_name);
```

Now queries that filter or search by `employee_name` may be able to use this index.

To remove an index:

```sql
DROP INDEX idx_employee_name;
```

**Database note:** Index creation and deletion syntax differs between database systems. For example, MySQL commonly uses `DROP INDEX index_name ON table_name`, while PostgreSQL uses `DROP INDEX index_name`.

## 3. Common Types of Indexes

### 1. B-tree Index

A B-tree index stores keys in a balanced tree structure.

It is useful for:

- Exact matches
- Range searches
- Sorting
- Comparisons such as `=`, `<`, `>`, and `BETWEEN`

B-tree indexes are common default index types in relational databases.

### 2. Hash Index

A hash index uses a hash function to locate values.

It is mainly useful for equality comparisons, such as:

```sql
SELECT *
FROM employees
WHERE employee_id = 100;
```

Hash indexes are generally not suitable for range searches or sorting.

Support and behaviour vary by database. PostgreSQL supports hash indexes, while MySQL's common InnoDB tables use B-tree indexes for ordinary indexes.

### 3. Unique Index

A unique index prevents duplicate values in the indexed column or column combination.

```sql
CREATE UNIQUE INDEX idx_employee_email
ON employees (email);
```

This prevents two rows from having the same email value.

A primary key is also enforced using a unique index in many relational database systems, though the implementation details vary.

### 4. Composite Index

A composite index contains more than one column.

```sql
CREATE INDEX idx_department_salary
ON employees (department_id, salary);
```

This can help queries that filter by `department_id`, or by `department_id` and `salary` together.

The order of columns matters. With a B-tree composite index on `(department_id, salary)`, queries using the first column are generally better supported than queries using only the second column.

### 5. Full-Text Index

A full-text index is designed to search words and text content.

It can be useful for searching articles, descriptions, or product information.

The syntax and supported features differ across databases. MySQL and PostgreSQL provide full-text search features, but they do not work in exactly the same way.

## 4. Clustered and Non-Clustered Indexes

These terms describe how table data relates to an index. Their exact meaning and implementation vary by database.

### Clustered Index

In systems such as SQL Server, a clustered index determines the order in which table rows are stored.

A table can have only one clustered index because its rows can have only one physical ordering.

In InnoDB, the table is organised around its primary key, which is called the clustered index.

### Non-Clustered Index

A non-clustered index is stored separately from the table rows. It contains indexed key values and information that helps locate the corresponding rows.

A table can have multiple non-clustered indexes.

## 5. Composite Index Column Order

Consider this index:

```sql
CREATE INDEX idx_department_salary
ON employees (department_id, salary);
```

It is generally useful for queries such as:

```sql
SELECT *
FROM employees
WHERE department_id = 10;
```

It can also help with:

```sql
SELECT *
FROM employees
WHERE department_id = 10
  AND salary > 40000;
```

However, a query that filters only by `salary` may not be able to use the same index efficiently because `department_id` is the leading column.

This is commonly called the **leftmost-prefix principle** for B-tree composite indexes.

## 6. Advantages of Indexes

- Faster searches for suitable queries.
- Better performance for some joins.
- Faster sorting and range filtering in supported cases.
- Unique indexes help enforce uniqueness.
- Can reduce the amount of table data that needs to be examined.

## 7. Disadvantages of Indexes

Indexes also have costs.

- They use additional storage.
- INSERT operations may become slower.
- UPDATE operations may need to update affected indexes.
- DELETE operations may need to remove index entries.
- Too many indexes can increase maintenance work.

Indexes should be created based on actual query patterns, not on every column.

## 8. When Should We Create an Index?

Indexes are often useful on columns that are frequently used in:

- WHERE conditions
- JOIN conditions
- ORDER BY clauses
- GROUP BY operations, depending on the query and database

However, an index is not guaranteed to improve every query.

For example, an index may not help much when:

- A query returns a large part of the table.
- The table is very small.
- The indexed column has low selectivity.
- The query applies a function or transformation that prevents use of the ordinary index, unless a suitable expression or functional index exists.

Use the database's query plan tools, such as `EXPLAIN`, to understand whether an index is being used.

## 9. Key Takeaways

- Indexes help the database locate rows efficiently.
- B-tree indexes support many common search and range operations.
- Hash indexes are mainly useful for equality searches where supported.
- Composite index column order matters.
- Indexes use storage and add work to data changes.
- Use EXPLAIN and real query patterns to decide which indexes are useful.

## 10. Conclusion

Indexes play an important role in improving database query performance. They help the database locate records faster without examining every row in a table.

However, indexes also require additional storage and can slow down INSERT, UPDATE, and DELETE operations. Therefore, indexes should be created carefully based on actual query requirements. Understanding indexes helps developers improve database performance while maintaining a balance between read and write operations.
