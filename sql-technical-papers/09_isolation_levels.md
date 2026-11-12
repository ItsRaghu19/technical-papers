# Technical Paper 09: Database Isolation Levels

**Subject:** Database Management Systems  
**Topic:** Transaction Isolation Levels  
**Focus:** Controlling the Effects of Concurrent Transactions

---

## 1. Introduction to Isolation Levels

Multiple transactions may run at the same time in a database.

For example, two users may update the same account, or one user may read data while another user changes it.

Isolation levels define how much one transaction is allowed to see the changes made by other concurrent transactions.

They help balance:

- Data correctness
- Concurrency
- Query performance
- Transaction waiting

The SQL standard defines four main isolation levels:

1. Read Uncommitted
2. Read Committed
3. Repeatable Read
4. Serializable

Database systems may implement these levels differently, so their exact behaviour can vary.

## 2. Common Concurrency Problems

Before looking at isolation levels, understand these three problems.

### 1. Dirty Read

A dirty read happens when a transaction reads a change made by another transaction that has not committed.

Example:

1. Transaction A changes a balance from ₹10,000 to ₹5,000.
2. Transaction B reads ₹5,000.
3. Transaction A rolls back.
4. Transaction B has read a value that was never committed.

### 2. Non-Repeatable Read

A non-repeatable read happens when a transaction reads the same row twice and gets different values because another transaction changed and committed that row between the reads.

Example:

1. Transaction A reads a balance of ₹10,000.
2. Transaction B changes it to ₹12,000 and commits.
3. Transaction A reads the same row again and gets ₹12,000.

### 3. Phantom Read

A phantom read happens when a transaction runs the same search condition twice and gets a different set of rows because another transaction inserted or deleted rows that match the condition.

Example:

1. Transaction A selects all employees with salary above ₹40,000.
2. Transaction B inserts an employee with salary ₹45,000 and commits.
3. Transaction A runs the same query again and sees an additional row.

## 3. Read Uncommitted

**Definition:** Read Uncommitted is the lowest standard isolation level. It allows a transaction to read changes made by another transaction before those changes are committed.

This means dirty reads are possible.

### Example

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN;

SELECT balance
FROM accounts
WHERE account_id = 1;

COMMIT;
```

If another transaction has made an uncommitted change, this transaction may be able to read it, depending on the database.

**Main point:** Read Uncommitted offers weak isolation and may expose temporary data.

## 4. Read Committed

**Definition:** Read Committed allows a transaction to read only committed data.

It prevents dirty reads.

However, another transaction may update and commit a row between two reads, so non-repeatable reads can still occur.

### Example

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN;

SELECT balance
FROM accounts
WHERE account_id = 1;

-- Another transaction may update and commit this row.

SELECT balance
FROM accounts
WHERE account_id = 1;

COMMIT;
```

The two SELECT statements may return different balances.

**Main point:** Read Committed prevents dirty reads, but it does not guarantee that repeated reads return the same value.

## 5. Repeatable Read

**Definition:** Repeatable Read ensures that rows read by a transaction remain consistent for repeated reads within that transaction.

Under the SQL standard, it prevents dirty reads and non-repeatable reads. The standard's treatment of phantom reads is different from Serializable.

### Example

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN;

SELECT balance
FROM accounts
WHERE account_id = 1;

-- Another transaction may try to update this row.

SELECT balance
FROM accounts
WHERE account_id = 1;

COMMIT;
```

Under a Repeatable Read implementation, the transaction's repeated read of the row sees a consistent value according to that database's rules.

**Database note:** Implementations differ. PostgreSQL uses a snapshot-based Repeatable Read level that also prevents phantom reads in the usual SQL-standard sense. InnoDB in MySQL uses its own Repeatable Read behaviour, including consistent snapshots and locking reads.

## 6. Serializable

**Definition:** Serializable is the strongest standard isolation level.

It makes concurrent transactions behave as if they had run one after another in some serial order.

This prevents the standard dirty read, non-repeatable read, and phantom read problems.

### Example

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

BEGIN;

SELECT COUNT(*)
FROM bookings
WHERE flight_id = 101;

-- Perform booking-related operations.

COMMIT;
```

If concurrent transactions cannot safely be treated as a serial order, the database may block one transaction or abort it with a serialization error.

The application may need to retry the transaction.

**Main point:** Serializable provides strong isolation, but it can reduce concurrency or require retries.

## 7. Isolation Levels Comparison

The following table shows the SQL standard's rules for the three common anomalies.

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| Read Uncommitted | Possible | Possible | Possible |
| Read Committed | Prevented | Possible | Possible |
| Repeatable Read | Prevented | Prevented | Possible under the standard |
| Serializable | Prevented | Prevented | Prevented |

Actual database behaviour can be stronger than the minimum standard behaviour.

## 8. Setting Isolation Levels

The exact syntax and scope differ between database systems.

For example, PostgreSQL supports:

```sql
BEGIN TRANSACTION
ISOLATION LEVEL SERIALIZABLE;

-- SQL statements

COMMIT;
```

MySQL also supports setting the isolation level for a session or transaction, with syntax and behaviour documented by its storage engine.

Check the documentation for the database being used before relying on a particular isolation level.

## 9. Choosing an Isolation Level

The correct isolation level depends on the application.

| Situation | Possible Choice | Reason |
|---|---|---|
| General application queries | Read Committed | Prevents dirty reads with useful concurrency |
| Repeated reads of the same data | Repeatable Read | Keeps repeated row reads consistent |
| Critical operations with complex rules | Serializable | Provides the strongest standard isolation |
| Workloads that tolerate weak isolation | Read Uncommitted | May allow more concurrency, but dirty reads are possible |

For important operations such as payments and bookings, choose isolation based on the full transaction logic. An isolation level alone does not replace constraints, correct queries, or suitable locking.

## 10. Key Takeaways

- Isolation levels control how concurrent transactions interact.
- Read Uncommitted may allow dirty reads.
- Read Committed prevents dirty reads.
- Repeatable Read also prevents non-repeatable reads under the SQL standard.
- Serializable provides the strongest standard isolation.
- Database products may implement isolation levels differently.

## 11. Conclusion

Isolation levels control how concurrent transactions interact with each other and how changes made by one transaction are visible to others. They help prevent problems such as dirty reads, non-repeatable reads, and phantom reads.

Choosing the correct isolation level depends on the application's requirements for data accuracy and performance. Understanding isolation levels helps developers manage concurrent transactions and build reliable database applications.
