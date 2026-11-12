# Technical Paper 04: Aggregations and Filters in SQL

**Subject:** Database Management Systems  
**Topic:** Aggregations and Filtering  
**Focus:** Summarising Data and Selecting Required Rows

---

## 1. Introduction

SQL provides several features to filter records and calculate useful summaries.

**Filtering** selects rows that match given conditions.

**Aggregation** calculates a summary from multiple rows, such as a total, average, or count.

These features are commonly used in reports, dashboards, and data analysis.

Consider an `employees` table:

| employee_id | employee_name | department | salary |
|---:|---|---|---:|
| 1 | Asha | Sales | 30000 |
| 2 | Ravi | IT | 45000 |
| 3 | Meera | Sales | 40000 |
| 4 | Kiran | IT | 50000 |
| 5 | Neha | HR | 35000 |

## 2. Filtering Rows with WHERE

The `WHERE` clause filters individual rows before grouping or aggregation takes place.

### Example: Find Employees with Salary Above 35,000

```sql
SELECT *
FROM employees
WHERE salary > 35000;
```

### Common Comparison Operators

| Operator | Meaning |
|---|---|
| `=` | Equal to |
| `<>` or `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

### Combining Conditions

Use `AND` when all conditions must be true.

```sql
SELECT *
FROM employees
WHERE department = 'Sales'
  AND salary > 35000;
```

Use `OR` when at least one condition must be true.

```sql
SELECT *
FROM employees
WHERE department = 'Sales'
   OR department = 'IT';
```

Use parentheses when combining AND and OR so the intended order is clear.

## 3. Other Useful Filtering Operators

### IN

Checks whether a value matches any value in a list.

```sql
SELECT *
FROM employees
WHERE department IN ('Sales', 'IT');
```

### BETWEEN

Checks whether a value falls within an inclusive range.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 30000 AND 45000;
```

### LIKE

Matches text using a pattern.

```sql
SELECT *
FROM employees
WHERE employee_name LIKE 'A%';
```

Common wildcards:

| Wildcard | Meaning |
|---|---|
| `%` | Zero or more characters |
| `_` | Exactly one character |

Case sensitivity depends on the database and its collation or comparison rules. PostgreSQL also provides `ILIKE` for case-insensitive pattern matching.

### IS NULL

Checks for missing or unknown values.

```sql
SELECT *
FROM employees
WHERE department IS NULL;
```

Do not use `= NULL`. SQL uses `IS NULL` and `IS NOT NULL` for NULL checks.

## 4. Aggregate Functions

Aggregate functions calculate one result from multiple input rows.

### COUNT()

Counts rows or non-NULL values, depending on the expression.

```sql
SELECT COUNT(*) AS employee_count
FROM employees;
```

`COUNT(*)` counts all rows. `COUNT(column_name)` counts only rows where that column is not NULL.

### SUM()

Calculates the total of numeric values.

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

### AVG()

Calculates the average of numeric values.

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

### MIN() and MAX()

Find the smallest and largest values.

```sql
SELECT
    MIN(salary) AS lowest_salary,
    MAX(salary) AS highest_salary
FROM employees;
```

### Aggregate Function Summary

| Function | Purpose |
|---|---|
| `COUNT()` | Counts rows or non-NULL values |
| `SUM()` | Calculates a total |
| `AVG()` | Calculates an average |
| `MIN()` | Finds the smallest value |
| `MAX()` | Finds the largest value |

Most aggregate functions ignore NULL values. `COUNT(*)` is the main exception because it counts rows, including rows with NULL columns.

## 5. GROUP BY

The `GROUP BY` clause puts rows with the same value into groups.

Aggregate functions can then calculate a result for each group.

### Example: Count Employees in Each Department

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

**Result:**

| department | employee_count |
|---|---:|
| Sales | 2 |
| IT | 2 |
| HR | 1 |

### Example: Average Salary by Department

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

### Grouping by Multiple Columns

```sql
SELECT
    department,
    job_title,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department, job_title;
```

This creates a group for each unique department and job-title combination.

## 6. HAVING

**Definition:** HAVING filters groups after the grouping and aggregate calculations have been performed.

### Example: Departments with More Than One Employee

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

### WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters groups |
| Applied before grouping | Applied after grouping |
| Commonly used without aggregates | Commonly used with aggregate conditions |

Example using both:

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE salary >= 30000
GROUP BY department
HAVING AVG(salary) > 35000;
```

First, rows with salary below 30,000 are removed. Then the remaining rows are grouped by department, and only groups with an average salary above 35,000 are returned.

## 7. ORDER BY

`ORDER BY` sorts the result.

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

- `ASC` sorts from low to high and is the default.
- `DESC` sorts from high to low.

You can sort by more than one column:

```sql
SELECT *
FROM employees
ORDER BY department ASC, salary DESC;
```

## 8. LIMIT and OFFSET

These clauses help return a smaller part of a result.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

This returns the three highest-paid employees.

To skip the first two rows:

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3 OFFSET 2;
```

**Database note:** PostgreSQL and MySQL support `LIMIT` and `OFFSET`. Other databases may use different syntax, such as `FETCH FIRST` or `TOP`.

Always use `ORDER BY` when the order of selected rows matters.

## 9. Logical Order of Query Processing

A useful simplified logical order for a SELECT query is:

1. `FROM` and `JOIN`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `ORDER BY`
7. `LIMIT` / `OFFSET`

This order helps explain why a SELECT alias may not be available in WHERE, and why HAVING can filter grouped results.

## 10. Key Takeaways

- WHERE filters individual rows.
- Aggregate functions summarise values.
- GROUP BY creates groups for calculations.
- HAVING filters grouped results.
- ORDER BY sorts query results.
- LIMIT and OFFSET help return a selected portion of a result.

## 11. Conclusion

Aggregate functions and filtering clauses help summarize, analyze, and retrieve meaningful information from database tables. Functions such as COUNT, SUM, AVG, MIN, and MAX make it easier to perform calculations on data.

Clauses such as WHERE, GROUP BY, and HAVING help filter individual records and grouped results. Understanding how these features work together is important for writing efficient queries and generating useful reports.
