# Technical Paper 05: Normalization in DBMS

**Subject:** Database Management Systems  
**Topic:** Database Normalization  
**Focus:** Organising Data and Reducing Duplication

---

## 1. Introduction to Normalization

Normalization is the process of organising data in database tables to reduce unnecessary duplication and avoid data problems.

A well-normalized database stores each fact in an appropriate place and connects related information using keys.

Normalization helps to:

- Reduce duplicate data.
- Improve data consistency.
- Make updates easier.
- Avoid insertion, update, and deletion anomalies.
- Maintain clear relationships between tables.

## 2. Problems Caused by Poor Database Design

Consider a table that stores student details and course details together:

| student_id | student_name | course_id | course_name | instructor |
|---:|---|---|---|---|
| 1 | Asha | 101 | SQL | Ravi |
| 1 | Asha | 102 | Python | Meera |
| 2 | Kiran | 101 | SQL | Ravi |

Student and course information is repeated in multiple rows.

This can cause three common problems.

### 1. Insertion Anomaly

An insertion anomaly occurs when we cannot add one type of information without adding unrelated information.

For example, we may want to add a new course before any student registers for it. If the table requires a student record, adding the course becomes difficult.

### 2. Update Anomaly

An update anomaly occurs when the same information is stored in multiple rows and must be changed in every location.

For example, if the SQL instructor changes, every row containing that course may need an update. Missing one row creates inconsistent data.

### 3. Deletion Anomaly

A deletion anomaly occurs when deleting one record also removes other information that we wanted to keep.

For example, deleting the only student enrolled in a course might also remove the only stored details about that course.

Normalization helps reduce these problems.

## 3. First Normal Form (1NF)

**Definition:** A table is in First Normal Form when each column contains a single value for each row, and there are no repeating groups of values.

### Example: Not in 1NF

| student_id | student_name | phone_numbers |
|---:|---|---|
| 1 | Asha | 9876543210, 9123456780 |
| 2 | Kiran | 9988776655 |

The `phone_numbers` column stores multiple values in one cell.

### Convert to 1NF

Store one phone number per row in a separate table:

| student_id | phone_number |
|---:|---|
| 1 | 9876543210 |
| 1 | 9123456780 |
| 2 | 9988776655 |

Now each cell contains a single value.

**Important points:**

- Each cell should contain one value.
- Repeating groups should be removed.
- Rows should be identifiable, usually through a primary key.

## 4. Second Normal Form (2NF)

**Definition:** A table is in Second Normal Form when:

1. It is already in 1NF.
2. Every non-key column depends on the whole candidate key, not just part of a composite key.

This rule mainly matters when a table has a composite key (a key made from more than one column).

### Example: Not in 2NF

Consider an enrollment table:

| student_id | course_id | student_name | course_name | grade |
|---:|---:|---|---|---|
| 1 | 101 | Asha | SQL | A |
| 1 | 102 | Asha | Python | B |
| 2 | 101 | Kiran | SQL | A |

Assume `(student_id, course_id)` is the composite primary key.

Dependencies:

- `student_name` depends only on `student_id`.
- `course_name` depends only on `course_id`.
- `grade` depends on both `student_id` and `course_id`.

The first two are partial dependencies, so the table is not in 2NF.

### Convert to 2NF

Create separate tables:

**students**

| student_id | student_name |
|---:|---|
| 1 | Asha |
| 2 | Kiran |

**courses**

| course_id | course_name |
|---:|---|
| 101 | SQL |
| 102 | Python |

**enrollments**

| student_id | course_id | grade |
|---:|---:|---|
| 1 | 101 | A |
| 1 | 102 | B |
| 2 | 101 | A |

Now student details depend on the student key, course details depend on the course key, and grade depends on the full enrollment key.

## 5. Third Normal Form (3NF)

**Definition:** A table is in Third Normal Form when:

1. It is already in 2NF.
2. Non-key columns do not depend transitively on a key through another non-key column.

In simple words, non-key columns should depend on the key, the whole key, and nothing but the key.

### Example: Not in 3NF

Consider an employee table:

| employee_id | employee_name | department_id | department_name |
|---:|---|---:|---|
| 1 | Asha | 10 | Sales |
| 2 | Ravi | 20 | IT |
| 3 | Meera | 10 | Sales |

Here:

- `employee_id` determines `department_id`.
- `department_id` determines `department_name`.

Therefore, `department_name` depends on `employee_id` through `department_id`. This is a transitive dependency.

### Convert to 3NF

**employees**

| employee_id | employee_name | department_id |
|---:|---|---:|
| 1 | Asha | 10 |
| 2 | Ravi | 20 |
| 3 | Meera | 10 |

**departments**

| department_id | department_name |
|---:|---|
| 10 | Sales |
| 20 | IT |

Now department information is stored separately.

## 6. Boyce-Codd Normal Form (BCNF)

**Definition:** BCNF is a stronger form of 3NF.

A table is in BCNF when every determinant (a column or group of columns that determines another value) is a superkey.

A superkey is a column or set of columns that uniquely identifies a row.

### Example

Suppose each instructor teaches only one course, but a course can be taught by multiple instructors in different departments.

Consider:

| student_id | course_id | instructor |
|---:|---:|---|
| 1 | 101 | Ravi |
| 2 | 101 | Ravi |
| 3 | 102 | Meera |

Assume the business rules include:

- A student can take multiple courses.
- Each instructor teaches only one course.
- A course can have multiple instructors.

The functional dependency `instructor → course_id` exists. If `instructor` is not a superkey, this dependency violates BCNF.

BCNF decomposition requires careful analysis of the actual functional dependencies and candidate keys. The exact decomposition depends on the full business rules.

## 7. Normal Forms Summary

| Normal Form | Main Rule |
|---|---|
| 1NF | Each cell contains a single value; remove repeating groups |
| 2NF | 1NF and no partial dependency on a composite candidate key |
| 3NF | 2NF and no transitive dependency of non-key attributes on a key |
| BCNF | Every determinant must be a superkey |

## 8. Normalization vs Denormalization

**Denormalization** intentionally adds some repeated or precomputed data to improve read performance or simplify reporting.

| Normalization | Denormalization |
|---|---|
| Reduces unnecessary duplication | May add controlled duplication |
| Helps prevent update anomalies | Can make reads or reports faster |
| Often uses more related tables | May use fewer joins |
| Common in transactional design | Common in reporting and analytics |

Denormalization should be planned carefully because repeated data must be kept in sync.

## 9. Key Takeaways

- Normalization organises data and reduces unnecessary duplication.
- 1NF removes repeating groups and multi-valued cells.
- 2NF removes partial dependencies.
- 3NF removes transitive dependencies of non-key attributes.
- BCNF requires every determinant to be a superkey.
- Denormalization may be useful when read performance is more important than avoiding all duplication.

## 10. Conclusion

Database normalization is an important process for organizing data into properly structured tables. It helps reduce data duplication, prevent insertion, update, and deletion anomalies, and maintain data integrity.

Understanding normalization forms, especially 1NF, 2NF, and 3NF, helps developers design databases that are easier to maintain and manage. A properly normalized database provides a strong foundation for reliable database applications.
