# Technical Paper 03: Joins in SQL

**Subject:** Database Management Systems  
**Topic:** SQL Joins  
**Focus:** Retrieving Related Data from Multiple Tables

---

## 1. Introduction to Joins

A database usually stores information in separate tables to reduce duplication and keep data organised.

For example, an application may store customer details in one table and their orders in another table.

A **JOIN** combines rows from two or more tables using a related column.

Joins help us retrieve related information without storing all details in one large table.

### Example Tables

**customers**

| customer_id | customer_name |
|---:|---|
| 1 | Asha |
| 2 | Ravi |
| 3 | Meera |

**orders**

| order_id | customer_id | amount |
|---:|---:|---:|
| 101 | 1 | 2500 |
| 102 | 1 | 1200 |
| 103 | 2 | 1800 |

The `customer_id` column connects the two tables.

## 2. INNER JOIN

**Definition:** INNER JOIN returns only rows that have matching values in both tables.

### Example

```sql
SELECT
    customers.customer_name,
    orders.order_id,
    orders.amount
FROM customers
INNER JOIN orders
    ON customers.customer_id = orders.customer_id;
```

**Result:**

| customer_name | order_id | amount |
|---|---:|---:|
| Asha | 101 | 2500 |
| Asha | 102 | 1200 |
| Ravi | 103 | 1800 |

Meera is not included because she has no matching order.

**Use cases:**

- Customers who have placed orders
- Employees who belong to a department
- Products that have matching category records

## 3. LEFT JOIN (LEFT OUTER JOIN)

**Definition:** LEFT JOIN returns every row from the left table and matching rows from the right table.

If there is no match, the right table's columns contain `NULL`.

### Example

```sql
SELECT
    customers.customer_name,
    orders.order_id,
    orders.amount
FROM customers
LEFT JOIN orders
    ON customers.customer_id = orders.customer_id;
```

**Result:**

| customer_name | order_id | amount |
|---|---:|---:|
| Asha | 101 | 2500 |
| Asha | 102 | 1200 |
| Ravi | 103 | 1800 |
| Meera | NULL | NULL |

Meera appears because LEFT JOIN keeps every row from the left table.

### Finding Customers Without Orders

```sql
SELECT customers.customer_name
FROM customers
LEFT JOIN orders
    ON customers.customer_id = orders.customer_id
WHERE orders.order_id IS NULL;
```

This query returns customers who have not placed an order.

## 4. RIGHT JOIN (RIGHT OUTER JOIN)

**Definition:** RIGHT JOIN returns every row from the right table and matching rows from the left table.

If there is no match, the left table's columns contain `NULL`.

### Example

```sql
SELECT
    customers.customer_name,
    orders.order_id,
    orders.amount
FROM customers
RIGHT JOIN orders
    ON customers.customer_id = orders.customer_id;
```

In this example, every order is returned, including an order even if it has no matching customer record.

A RIGHT JOIN can often be rewritten as a LEFT JOIN by swapping the table order.

## 5. FULL OUTER JOIN

**Definition:** FULL OUTER JOIN returns all matching and non-matching rows from both tables.

Where a row has no match, the columns from the other table contain `NULL`.

### Example

```sql
SELECT
    customers.customer_name,
    orders.order_id,
    orders.amount
FROM customers
FULL OUTER JOIN orders
    ON customers.customer_id = orders.customer_id;
```

This includes:

- Customers with matching orders.
- Customers without orders.
- Orders without matching customers.

**Database note:** PostgreSQL supports FULL OUTER JOIN directly. MySQL does not support the `FULL OUTER JOIN` keyword directly. It can be built using a combination of LEFT JOIN and RIGHT JOIN with `UNION`.

## 6. CROSS JOIN

**Definition:** CROSS JOIN returns every possible combination of rows from the two tables.

If the first table has 3 rows and the second table has 2 rows, the result has 6 rows.

### Example

```sql
SELECT
    customers.customer_name,
    products.product_name
FROM customers
CROSS JOIN products;
```

If there are 3 customers and 4 products, the result contains 12 combinations.

**Use cases:**

- Generating combinations
- Creating test data
- Pairing every size with every colour

Use CROSS JOIN carefully because the result can become very large.

## 7. SELF JOIN

**Definition:** A SELF JOIN joins a table with itself.

It is useful when rows in the same table are related to other rows in that table.

### Example

Consider an `employees` table:

| employee_id | employee_name | manager_id |
|---:|---|---:|
| 1 | Priya | NULL |
| 2 | Arjun | 1 |
| 3 | Neha | 1 |
| 4 | Kiran | 2 |

To display each employee with their manager:

```sql
SELECT
    employee.employee_name AS employee,
    manager.employee_name AS manager
FROM employees AS employee
LEFT JOIN employees AS manager
    ON employee.manager_id = manager.employee_id;
```

The table is given two aliases so SQL can treat it as two separate roles.

## 8. Joining More Than Two Tables

A query can join several tables when the relationships are available.

Suppose an application has:

- `customers`
- `orders`
- `order_items`
- `products`

Example:

```sql
SELECT
    customers.customer_name,
    orders.order_id,
    products.product_name,
    order_items.quantity
FROM customers
INNER JOIN orders
    ON customers.customer_id = orders.customer_id
INNER JOIN order_items
    ON orders.order_id = order_items.order_id
INNER JOIN products
    ON order_items.product_id = products.product_id;
```

This query displays the customer, order, product, and quantity together.

## 9. Important Points About Joins

- Use clear table aliases when queries become long.
- Always check that the JOIN condition uses the correct related columns.
- A missing or incorrect JOIN condition can create many unwanted row combinations.
- One-to-many relationships can produce multiple result rows for one row in the first table.
- `NULL` values may appear when an outer join has no match.
- A JOIN does not automatically remove duplicate-looking rows. The result depends on the matching rows and selected columns.

## 10. Join Summary

| Join | What It Returns |
|---|---|
| INNER JOIN | Matching rows from both tables |
| LEFT JOIN | All left rows and matching right rows |
| RIGHT JOIN | All right rows and matching left rows |
| FULL OUTER JOIN | All rows from both tables |
| CROSS JOIN | Every possible row combination |
| SELF JOIN | Rows joined to other rows in the same table |

## 11. Key Takeaways

- JOINs retrieve related data stored in different tables.
- INNER JOIN returns only matching rows.
- Outer joins preserve non-matching rows from one or both tables.
- CROSS JOIN creates every possible combination.
- SELF JOIN compares rows within the same table.
- Correct JOIN conditions are important for accurate results.

## 12. Conclusion

SQL Joins are important for retrieving related information from multiple tables. They help combine data using common columns and relationships between tables.

Understanding different types of joins, such as INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL OUTER JOIN, makes it easier to write queries for real-world database operations. Joins are widely used in reporting, data analysis, and application development.
