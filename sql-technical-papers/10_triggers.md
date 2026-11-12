# Technical Paper 10: Triggers in SQL

**Subject:** Database Management Systems  
**Topic:** SQL Triggers  
**Focus:** Automatically Running Actions When Database Events Occur

---

## 1. Introduction to Triggers

A trigger is a database object that automatically runs when a specified event occurs on a table or view.

Triggers are commonly used to:

- Record changes in audit tables.
- Validate or control data changes.
- Maintain related information.
- Track important database events.

Unlike a normal SQL statement, a trigger does not usually need to be called directly by the user. The database runs it when its defined event occurs.

## 2. Types of Trigger Events

Triggers can be associated with events such as:

- `INSERT`
- `UPDATE`
- `DELETE`

For example:

- An INSERT trigger can record when a new employee is added.
- An UPDATE trigger can store the previous and new salary.
- A DELETE trigger can record which row was removed.

The available events and trigger features depend on the database system.

## 3. BEFORE Trigger

A BEFORE trigger runs before the database completes the triggering operation.

It can be used to check or adjust values before they are stored.

### Example: Validate a Salary

The following is a PostgreSQL example:

```sql
CREATE OR REPLACE FUNCTION check_employee_salary()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.salary < 0 THEN
        RAISE EXCEPTION 'Salary cannot be negative';
    END IF;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER employee_salary_check
BEFORE INSERT OR UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION check_employee_salary();
```

Here:

- `NEW` represents the new row values.
- The function checks whether the salary is negative.
- If it is negative, the operation is rejected.

This example uses PostgreSQL's PL/pgSQL syntax. Other databases use different trigger syntax.

## 4. AFTER Trigger

An AFTER trigger runs after the triggering operation has completed its main row operation.

It is often used for tasks such as recording changes in an audit table.

### Example: Record a New Employee

PostgreSQL example:

```sql
CREATE TABLE employee_audit (
    audit_id SERIAL PRIMARY KEY,
    employee_id INT,
    action_type VARCHAR(20),
    action_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Create a trigger function:

```sql
CREATE OR REPLACE FUNCTION log_employee_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO employee_audit (
        employee_id,
        action_type
    )
    VALUES (
        NEW.employee_id,
        'INSERT'
    );

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Create the trigger:

```sql
CREATE TRIGGER employee_insert_audit
AFTER INSERT ON employees
FOR EACH ROW
EXECUTE FUNCTION log_employee_insert();
```

Whenever a new employee is inserted, the trigger adds a record to `employee_audit`.

In PostgreSQL, the trigger action normally runs as part of the same transaction. If the transaction rolls back, the audit insert rolls back too.

## 5. Row-Level and Statement-Level Triggers

### Row-Level Trigger

A row-level trigger runs once for each affected row.

For example, if an UPDATE statement changes five rows, a row-level trigger runs five times.

In PostgreSQL, row-level triggers use `FOR EACH ROW`.

### Statement-Level Trigger

A statement-level trigger runs once for the SQL statement, even if the statement affects many rows or no rows.

In PostgreSQL, statement-level triggers use `FOR EACH STATEMENT`.

### Comparison

| Feature | Row-Level Trigger | Statement-Level Trigger |
|---|---|---|
| Runs | Once per affected row | Once per SQL statement |
| Can inspect each row's old and new values | Yes, where supported | Not directly in the same way |
| Useful for | Row-specific checks and audit records | Actions related to the statement as a whole |

Trigger types and available transition data vary by database.

## 6. OLD and NEW Values

Many database systems provide special references to row values inside triggers.

- `OLD` represents the row values before an UPDATE or DELETE.
- `NEW` represents the row values after an INSERT or UPDATE.

For example, an UPDATE trigger can compare the previous salary with the new salary.

PostgreSQL example:

```sql
CREATE OR REPLACE FUNCTION log_salary_change()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO salary_audit (
        employee_id,
        old_salary,
        new_salary
    )
    VALUES (
        OLD.employee_id,
        OLD.salary,
        NEW.salary
    );

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

This can be connected to a BEFORE or AFTER UPDATE trigger, depending on the intended behaviour.

`OLD` and `NEW` are not available in exactly the same way for every event. For example, an INSERT has a NEW row but no OLD row.

## 7. Dropping a Trigger

A trigger can be removed when it is no longer required.

In PostgreSQL:

```sql
DROP TRIGGER employee_insert_audit ON employees;
```

The trigger function is a separate database object and may be removed separately if it is no longer used:

```sql
DROP FUNCTION log_employee_insert();
```

Check dependencies before dropping a function.

## 8. Advantages of Triggers

- Automatically run when defined events occur.
- Can help maintain audit records.
- Can enforce certain data rules close to the database.
- Can keep related database information in sync.
- Reduce the need to repeat the same action in many application code paths.

## 9. Disadvantages of Triggers

- Their execution may not be obvious to someone reading the application code.
- Complex triggers can make debugging difficult.
- Too many triggers can affect performance.
- A trigger can cause additional database operations during an INSERT, UPDATE, or DELETE.
- Trigger behaviour can differ between database systems.

Triggers should be kept small, clear, and well documented.

## 10. Triggers vs Stored Procedures

| Triggers | Stored Procedures |
|---|---|
| Run automatically when a defined event occurs | Usually called explicitly by an application or user |
| Associated with a table, view, or database event | Stored as a callable database routine |
| Useful for automatic event-based actions | Useful for reusable database operations |
| The triggering statement causes them to run | A CALL or database-specific command invokes them |

The syntax and capabilities of stored procedures and triggers vary by database.

## 11. Key Takeaways

- Triggers run automatically when defined database events occur.
- Common events include INSERT, UPDATE, and DELETE.
- BEFORE triggers run before the main operation completes.
- AFTER triggers run after the main operation.
- Row-level triggers run for each affected row; statement-level triggers run once per statement.
- OLD and NEW provide access to row values where supported.
- Keep triggers simple and document their behaviour.

## 12. Conclusion

Triggers allow databases to execute specific actions automatically when events such as INSERT, UPDATE, or DELETE occur. They are useful for maintaining audit records, enforcing certain rules, and keeping related data updated.

However, triggers should be designed carefully because complex triggers can make database behaviour difficult to understand and debug. Understanding when and how to use triggers helps developers automate database operations while keeping database logic clear and maintainable.
