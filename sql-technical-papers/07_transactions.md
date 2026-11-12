# Technical Paper 07: Transactions in SQL

**Subject:** Database Management Systems  
**Topic:** SQL Transactions  
**Focus:** Managing a Group of Database Operations

---

## 1. Introduction to Transactions

A transaction is a group of database operations treated as one logical unit of work.

A transaction can contain one or more SQL statements, such as INSERT, UPDATE, and DELETE.

Transactions help keep data correct when several related operations must succeed together.

### Example

Consider transferring ₹5,000 from Account A to Account B.

The transaction requires two operations:

1. Deduct ₹5,000 from Account A.
2. Add ₹5,000 to Account B.

Both operations should succeed together. If one fails, the database should undo the uncommitted changes.

## 2. Starting a Transaction

A transaction can be started using `BEGIN` or `START TRANSACTION`, depending on the database.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 5000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 5000
WHERE account_id = 2;

COMMIT;
```

`COMMIT` makes the transaction's changes permanent.

If an error occurs before the commit, the application can issue `ROLLBACK` to cancel the transaction.

```sql
ROLLBACK;
```

## 3. COMMIT

**Definition:** COMMIT saves the changes made by the current transaction.

Example:

```sql
BEGIN;

INSERT INTO customers (customer_id, customer_name)
VALUES (10, 'Asha');

COMMIT;
```

After a successful commit, the inserted row becomes part of the committed database state.

## 4. ROLLBACK

**Definition:** ROLLBACK cancels changes made by the current uncommitted transaction.

Example:

```sql
BEGIN;

UPDATE products
SET price = 1500
WHERE product_id = 1;

ROLLBACK;
```

The update is cancelled, so the previous committed price remains.

A rollback cannot normally undo a transaction that has already been committed. That requires a separate corrective transaction, backup restoration, or another recovery method.

## 5. SAVEPOINT

A SAVEPOINT creates a point inside a transaction to which we can roll back without cancelling the entire transaction.

### Example

```sql
BEGIN;

INSERT INTO customers (customer_id, customer_name)
VALUES (11, 'Ravi');

SAVEPOINT customer_added;

INSERT INTO customers (customer_id, customer_name)
VALUES (12, 'Meera');

ROLLBACK TO SAVEPOINT customer_added;

COMMIT;
```

The second insert is undone, but the first insert remains and is committed.

SAVEPOINT support and exact syntax can vary slightly between databases.

## 6. Transaction States

A transaction moves through different states during its lifetime.

| State | Meaning |
|---|---|
| Active | The transaction is executing |
| Partially committed | Its final statement has executed, but commit processing is not complete |
| Committed | The transaction has completed successfully |
| Failed | The transaction cannot continue because of an error |
| Aborted | Its changes have been rolled back |

The exact state model is often explained in database theory. A database product may expose different internal details.

## 7. Autocommit

Autocommit is a mode in which each statement is committed automatically when it succeeds, unless an explicit transaction is active.

For example, in an autocommit session:

```sql
UPDATE products
SET price = 2000
WHERE product_id = 1;
```

The statement may be committed automatically.

To group multiple statements into one transaction, explicitly start a transaction.

Autocommit defaults and commands for changing it differ between database systems and client tools.

## 8. Transactions and ACID

Transactions are the units of work to which ACID properties apply.

| ACID Property | Role in a Transaction |
|---|---|
| Atomicity | All operations succeed or the transaction is rolled back |
| Consistency | Database rules remain valid |
| Isolation | Concurrent transactions are controlled |
| Durability | Committed changes survive failures |

## 9. Important Transaction Rules

- Keep transactions as short as practical.
- Commit when all required operations succeed.
- Roll back when the operation cannot safely continue.
- Avoid waiting for user input while a transaction is open.
- Handle errors in application code.
- Be prepared to retry transactions that fail because of deadlocks or serialization conflicts, where appropriate.
- Do not assume a database rollback can undo actions performed by an external service.

## 10. Key Takeaways

- Transactions group related SQL operations.
- BEGIN starts a transaction.
- COMMIT saves its changes.
- ROLLBACK cancels uncommitted changes.
- SAVEPOINT allows a partial rollback.
- Short, well-managed transactions help reduce locking and concurrency problems.

## 11. Conclusion

Transactions allow multiple database operations to be handled as a single logical unit of work. Commands such as BEGIN, COMMIT, ROLLBACK, and SAVEPOINT help control how these operations are executed.

Understanding transactions is important for maintaining data accuracy and preventing partial updates. They are especially useful in applications involving banking, payments, orders, and other operations where data reliability is essential.
