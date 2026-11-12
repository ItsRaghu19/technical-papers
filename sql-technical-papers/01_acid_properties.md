# Technical Paper 01: ACID Properties in DBMS

**Subject:** Database Management Systems  
**Topic:** ACID Properties  
**Focus:** SQL Transactions and Data Reliability

---

## 1. Introduction to ACID Properties

ACID is a set of four fundamental properties that ensure reliable and consistent database transactions.

ACID stands for:

1. **Atomicity**
2. **Consistency**
3. **Isolation**
4. **Durability**

These properties ensure that database transactions are processed reliably, even when errors, system crashes, or concurrent operations occur.

### Why Do We Need ACID Properties?

Consider a banking application where a user transfers ₹5,000 from Account A to Account B.

The transaction involves two operations:

- Deduct ₹5,000 from Account A.
- Add ₹5,000 to Account B.

Suppose the amount is deducted from Account A, but the application crashes before adding it to Account B.

Without proper transaction management, the money could disappear from the system.

ACID properties help prevent such situations by ensuring that database transactions remain reliable.

## 2. Atomicity

**Definition:** Atomicity ensures that a transaction is treated as a single, indivisible unit of work.

Either all operations within a transaction are completed successfully, or none of them are applied.

This is commonly described as **All or Nothing**.

### Example

Consider transferring ₹5,000 from Account A to Account B.

Initial balances:

| Account | Balance |
|---|---:|
| Account A | ₹20,000 |
| Account B | ₹10,000 |

Transaction:

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

If both operations succeed, the transaction is committed.

Updated balances:

| Account | Balance |
|---|---:|
| Account A | ₹15,000 |
| Account B | ₹15,000 |

### What Happens If an Error Occurs?

Suppose the first operation succeeds, but the second operation fails.

The database can execute:

```sql
ROLLBACK;
```

This cancels the uncommitted transaction and restores the original balances.

### Important SQL Commands

| Command | Purpose |
|---|---|
| `BEGIN` | Starts a transaction |
| `COMMIT` | Saves the transaction changes |
| `ROLLBACK` | Cancels uncommitted changes |

**Real-world applications:**

- Banking transactions
- Online payments
- Ticket booking
- E-commerce orders

## 3. Consistency

**Definition:** Consistency ensures that a transaction moves the database from one valid state to another valid state.

A transaction must follow the defined database rules, constraints, and integrity requirements.

These rules include:

- Primary key constraints
- Foreign key constraints
- Unique constraints
- NOT NULL constraints
- CHECK constraints

### Example

Suppose a bank account must always maintain a non-negative balance.

We can enforce this rule using a CHECK constraint:

```sql
CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    account_name VARCHAR(100),
    balance DECIMAL(10, 2)
        CHECK (balance >= 0)
);
```

Now consider the following operation:

```sql
UPDATE accounts
SET balance = balance - 25000
WHERE account_id = 1;
```

If the account balance is ₹20,000, this operation would produce a negative balance.

The database rejects the operation because it violates the CHECK constraint.

Therefore, the transaction cannot successfully commit in an invalid state.

### Important Points

- Consistency maintains database integrity.
- Every successful transaction must satisfy the applicable constraints.
- Transactions must follow business rules enforced by the application or database.
- ACID consistency does not automatically guarantee that every business rule has been correctly defined.

**Real-world applications:**

- Preventing negative account balances
- Maintaining unique email addresses
- Ensuring valid product quantities
- Maintaining valid relationships between tables

## 4. Isolation

**Definition:** Isolation ensures that concurrent transactions do not interfere with each other in a way that produces incorrect results.

Multiple users can access and modify a database simultaneously.

Without isolation, one transaction might read incomplete or inconsistent data modified by another transaction.

### Example

Suppose two users are trying to book the last available seat on a flight.

Initially:

```text
Available Seats = 1
```

Two transactions execute simultaneously.

**Transaction A:**

```sql
BEGIN;

SELECT available_seats
FROM flights
WHERE flight_id = 101;

UPDATE flights
SET available_seats = available_seats - 1
WHERE flight_id = 101;

COMMIT;
```

**Transaction B:**

```sql
BEGIN;

SELECT available_seats
FROM flights
WHERE flight_id = 101;

UPDATE flights
SET available_seats = available_seats - 1
WHERE flight_id = 101;

COMMIT;
```

If both transactions read the initial value of 1 before either update takes effect, both users might believe the seat is available.

This could lead to overbooking if the application does not coordinate the operations correctly.

Database isolation mechanisms help prevent these concurrency problems.

### Common Concurrency Problems

#### 1. Dirty Read

A transaction reads data modified by another transaction that has not yet committed.

Example:

1. Transaction A changes a balance from ₹10,000 to ₹5,000.
2. Transaction B reads ₹5,000.
3. Transaction A rolls back.
4. Transaction B has read a value that was never committed.

#### 2. Non-Repeatable Read

A transaction reads the same row twice but gets different values because another transaction modified and committed that row between the reads.

#### 3. Phantom Read

A transaction executes the same query twice but receives a different set of rows because another transaction inserted or deleted rows matching the query condition.

**Real-world applications:**

- Online banking
- Flight reservations
- Inventory management
- Online shopping

## 5. Durability

**Definition:** Durability ensures that once a transaction is successfully committed, its changes remain stored, even if the database crashes or the system loses power.

### Example

Suppose a user transfers ₹5,000 successfully.

The database executes:

```sql
COMMIT;
```

Immediately after the transaction is committed, the database server crashes.

When the server restarts, the transaction should still be reflected in the database.

Account balances should remain:

| Account | Balance |
|---|---:|
| Account A | ₹15,000 |
| Account B | ₹15,000 |

The database uses mechanisms such as:

- Write-Ahead Logging (WAL)
- Transaction logs
- Recovery mechanisms
- Persistent storage

These mechanisms help recover committed transactions after failures.

**Real-world applications:**

- Banking systems
- Payment processing
- Order management
- Financial record keeping

## 6. ACID Properties Summary

| Property | Meaning | Main Purpose |
|---|---|---|
| Atomicity | All or nothing | Prevent partial transactions |
| Consistency | Valid state to valid state | Maintain data integrity |
| Isolation | Transactions do not improperly interfere | Handle concurrent operations |
| Durability | Committed changes persist | Protect committed data |

## 7. ACID Properties in a Real-World Scenario

Consider an e-commerce application where a customer purchases a product.

The transaction involves:

1. Deducting the product quantity from inventory.
2. Creating an order record.
3. Recording the payment status.

How does ACID help?

| Property | Implementation |
|---|---|
| Atomicity | All database operations succeed or the database transaction rolls back |
| Consistency | Inventory and other database constraints are maintained |
| Isolation | Concurrent purchases do not incorrectly update inventory |
| Durability | A successfully committed order remains recorded after a crash |

**Important:** External payment gateways are separate systems. A database rollback cannot automatically reverse an external payment. Such workflows need additional mechanisms, such as payment reversals or distributed transaction patterns.

## 8. Key Takeaways

- ACID provides reliability for database transactions.
- Atomicity follows the all-or-nothing principle.
- Consistency maintains database integrity.
- Isolation controls interference between concurrent transactions.
- Durability ensures committed changes survive failures.
- SQL transaction commands such as `BEGIN`, `COMMIT`, and `ROLLBACK` help implement transaction management.

## 9. Conclusion

ACID properties are essential for maintaining reliable and consistent database transactions. They ensure that transactions are completed correctly, database rules are maintained, concurrent operations do not interfere improperly, and committed changes are not lost.

Understanding ACID properties helps developers build reliable applications, especially in systems that handle important data, such as banking, payments, and online shopping.
