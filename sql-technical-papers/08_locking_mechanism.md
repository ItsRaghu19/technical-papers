# Technical Paper 08: Locking Mechanisms in DBMS

**Subject:** Database Management Systems  
**Topic:** Locking Mechanisms  
**Focus:** Controlling Concurrent Access to Data

---

## 1. Introduction to Locking

A database may receive requests from many users at the same time.

For example, two customers may try to purchase the last item in stock. If both operations read the same stock quantity and update it without coordination, the database may produce an incorrect result.

A **lock** is a mechanism that controls access to data while transactions are using it.

Locks help prevent conflicting operations from interfering with one another.

## 2. Why Are Locks Needed?

Suppose a product has one unit in stock.

Two transactions try to buy it at the same time:

1. Transaction A reads the stock as 1.
2. Transaction B also reads the stock as 1.
3. Both transactions try to reduce the stock by 1.

Without suitable concurrency control, both customers might be told that the purchase succeeded.

Locking helps coordinate these operations so that conflicting changes do not both proceed as if they were the only transaction.

## 3. Shared Lock (S Lock)

A shared lock is used when a transaction needs to read data.

Multiple transactions can usually hold shared locks on the same data at the same time.

This allows concurrent reads.

### Example

- Transaction A reads an account balance.
- Transaction B also reads the same account balance.
- Both can hold shared locks where the database uses this locking approach.

A shared lock generally prevents another transaction from obtaining an incompatible exclusive lock on the same locked resource.

## 4. Exclusive Lock (X Lock)

An exclusive lock is used when a transaction needs to modify data.

Only one transaction can hold an exclusive lock on a given resource at a time.

An exclusive lock prevents other transactions from taking incompatible locks on that resource.

### Example

```sql
BEGIN;

UPDATE products
SET stock = stock - 1
WHERE product_id = 101;

COMMIT;
```

The database obtains the necessary locks to protect the update. The exact lock type and scope depend on the database engine and execution plan.

## 5. Shared Lock vs Exclusive Lock

| Feature | Shared Lock | Exclusive Lock |
|---|---|---|
| Main purpose | Read protection | Write protection |
| Multiple transactions | Compatible shared locks may coexist | Only one exclusive lock per resource |
| Allows other reads | Usually yes | Usually blocks conflicting reads |
| Allows other writes | No, if they need an incompatible lock | No |

The exact compatibility rules depend on the database's locking system.

## 6. Row-Level and Table-Level Locks

### Row-Level Locking

A row-level lock protects a specific row.

It allows transactions to work on different rows of the same table at the same time, where their operations do not conflict.

**Example:** One transaction updates product 101 while another updates product 202.

### Table-Level Locking

A table-level lock protects a whole table.

It can block more operations than a row-level lock, but it may be useful for operations that need broad protection.

### Comparison

| Feature | Row-Level Lock | Table-Level Lock |
|---|---|---|
| Lock scope | Specific rows | Entire table |
| Concurrent work | Often allows more parallel work | Can restrict more operations |
| Lock management | May require more lock tracking | Fewer lock entries |
| Common use | Individual record updates | Broad table operations |

Not every database uses exactly the same lock types or behaviour.

## 7. Intent Locks

Some database systems use intent locks to show that a transaction plans to lock rows or other smaller parts of a table.

For example, a transaction may hold an intent-exclusive lock at the table level before taking exclusive locks on individual rows.

Intent locks help the database manage conflicts between table-level and row-level locks.

They are an implementation detail in systems such as SQL Server and InnoDB.

## 8. Deadlocks

**Definition:** A deadlock occurs when two or more transactions wait for one another's locks, and none can continue.

### Example

- Transaction A locks Row 1.
- Transaction B locks Row 2.
- Transaction A requests Row 2 and waits for B.
- Transaction B requests Row 1 and waits for A.

Both transactions are waiting for the other to release a lock.

This creates a deadlock.

### How Do Databases Handle Deadlocks?

Databases commonly detect deadlocks and choose one transaction as a victim.

The victim transaction is rolled back so that the other transaction can continue.

### How Can We Reduce Deadlocks?

- Access rows in a consistent order.
- Keep transactions short.
- Avoid unnecessary locks.
- Use suitable indexes so statements do not examine or lock more rows than needed.
- Handle deadlock errors and retry safely when appropriate.

## 9. Locking and Performance

Locks protect data, but they can also affect performance.

Long-running transactions may hold locks for a long time. Other transactions may have to wait.

This can cause:

- Blocking
- Longer response times
- Reduced concurrency
- Deadlocks

A good database design uses appropriate transactions and queries to reduce unnecessary waiting.

## 10. Locking vs MVCC

Many modern databases also use **Multi-Version Concurrency Control (MVCC)**.

MVCC keeps different versions of rows so that readers can often access a consistent version without blocking writers.

For example, PostgreSQL uses MVCC for ordinary reads under its common isolation levels.

MVCC does not remove all locks. Databases still use locks for conflicting writes, schema changes, and other operations.

| Locking | MVCC |
|---|---|
| Controls access using locks | Maintains multiple row versions |
| Conflicting operations may wait | Readers can often read an older consistent version |
| Used to protect concurrent changes | Helps readers and writers work with less blocking |
| Still used in MVCC databases | Does not eliminate the need for locks |

## 11. Key Takeaways

- Locks control concurrent access to database resources.
- Shared locks are commonly associated with reading.
- Exclusive locks are commonly associated with modifying data.
- Row-level locks protect selected rows; table-level locks protect a whole table.
- Deadlocks happen when transactions wait for one another's locks.
- MVCC reduces some blocking but does not remove the need for locks.

## 12. Conclusion

Locking mechanisms help manage simultaneous access to database records. They prevent conflicting operations from modifying the same data in an unsafe way and help maintain data consistency.

Understanding different locking mechanisms is important when developing applications that handle multiple users and concurrent transactions. Proper use of locks helps prevent data conflicts while allowing transactions to work safely.
