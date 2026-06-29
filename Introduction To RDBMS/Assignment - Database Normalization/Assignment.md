# DBMS Normalization - Assignment Solution

## Question 1

### Observe the structure of the given EMPLOYEE table and suggest whether it can be further normalized.

**Table Structure**

`EMPNO, ENAME, SAL, DEPTNO, DNAME, LOC`

### Answer

### Current Normal Form: Second Normal Form (2NF)

### Explanation

- `EMPNO` is the **Primary Key** of the table.
- `ENAME` and `SAL` are fully and directly dependent on `EMPNO`, which is perfectly fine.
- `DEPTNO` is also dependent on `EMPNO`, so it correctly belongs in this table.
- The problem arises with `DNAME` and `LOC` — these two attributes describe department information and are functionally dependent on `DEPTNO`, not on `EMPNO`.
- Since a non-key attribute (`DNAME`, `LOC`) depends on another non-key attribute (`DEPTNO`), this is a classic case of **Transitive Dependency**.
- As long as this dependency exists, the table cannot qualify as **Third Normal Form (3NF)**.

### Converting the Table into 3NF

To eliminate the transitive dependency, department-related columns must be moved into their own dedicated table, linked back through `DEPTNO` as a foreign key.

#### EMPLOYEE Table

| EMPNO | ENAME | SAL | DEPTNO |
|------:|-------|----:|-------:|
| 101 | Ram | 30000 | 10 |

#### DEPARTMENT Table

| DEPTNO | DNAME | LOC |
|-------:|-------|-----|
| 10 | Sales | Delhi |

### Result

With `DNAME` and `LOC` moved to the DEPARTMENT table, every non-key attribute in each table now depends solely on its own primary key. The transitive dependency is gone and both tables satisfy **Third Normal Form (3NF)**.

---

## Question 2

### Observe the structure of the given STUDENT table and suggest whether it can be further normalized.

**Table Structure**

`ROLLNO, NAME, AGE, EXAM, MARKS, GRADE`

### Answer

### Current Normal Form: Second Normal Form (2NF)

### Explanation

- `ROLLNO` is the **Primary Key** of this table.
- `NAME`, `AGE`, `EXAM`, and `MARKS` are all attributes that relate to a student and can be considered associated with `ROLLNO`.
- However, `GRADE` is not determined by `ROLLNO` directly — instead, the grade is assigned based on the `MARKS` scored by the student.
- This forms an indirect dependency chain: `ROLLNO → MARKS → GRADE`, which is a **Transitive Dependency**.
- Because of this, the table violates **Third Normal Form (3NF)** and needs to be restructured.

### Converting the Table into 3NF

The student's personal details, exam result, and grading criteria should each be stored in their own separate tables.

#### STUDENT Table

| ROLLNO | NAME | AGE |
|-------:|------|----:|
| 1 | Aaliya | 20 |

#### RESULT Table

| ROLLNO | EXAM | MARKS |
|-------:|------|------:|
| 1 | DBMS | 95 |

#### GRADE Table

| MARKS | GRADE |
|------:|-------|
| 95 | A+ |

### Result

By splitting the data across three tables, the transitive dependency between `MARKS` and `GRADE` is completely removed. Every non-key attribute now depends only on the primary key of its respective table, satisfying **Third Normal Form (3NF)**.

---

## Question 3

### Observe the structure of the given EMPLOYEE table and identify the problem. Also suggest how to solve it.

**Table Structure**

`EMPNO, PROJECT_NO, NO_OF_DAYS, CUSTOMERNAME`

**Note:** `EMPNO` and `PROJECT_NO` together form a **Composite Primary Key**.

### Answer

### Problem Identified: Partial Dependency

### Explanation

- The **Composite Primary Key** of this table is the combination of `EMPNO` and `PROJECT_NO`.
- `NO_OF_DAYS` tells us how many days a particular employee worked on a particular project — so it logically requires both `EMPNO` and `PROJECT_NO` to be identified. This is a full dependency and is correct.
- `CUSTOMERNAME`, on the other hand, is only connected to `PROJECT_NO`. No matter which employee is assigned, the customer name stays the same for a given project. This means `CUSTOMERNAME` depends on only one part of the composite key.
- A non-key attribute depending on just a portion of the composite primary key is called a **Partial Dependency**.
- Because of this partial dependency, the table is stuck at **First Normal Form (1NF)** and must be upgraded to 2NF.

### Converting the Table into 2NF

Divide the table into two parts — one that captures the employee-project relationship, and another that holds project-specific details independently.

#### EMPLOYEE_PROJECT Table

| EMPNO | PROJECT_NO | NO_OF_DAYS |
|------:|------------|----------:|
| 101 | P101 | 20 |

#### PROJECT Table

| PROJECT_NO | CUSTOMERNAME |
|------------|--------------|
| P101 | ABC Ltd |

### Result

Once `CUSTOMERNAME` is moved to the PROJECT table where `PROJECT_NO` acts as the primary key, the partial dependency is fully resolved. All non-key attributes are now dependent on the complete primary key of their respective tables, bringing the design up to **Second Normal Form (2NF)**.

---

# Conclusion

All three tables in this assignment contained dependency issues that prevented them from reaching a higher normal form:

- **Question 1** had a **Transitive Dependency** — `DNAME` and `LOC` depended on `DEPTNO` rather than directly on `EMPNO`.
- **Question 2** had a **Transitive Dependency** — `GRADE` was determined by `MARKS` rather than directly by `ROLLNO`.
- **Question 3** had a **Partial Dependency** — `CUSTOMERNAME` depended only on `PROJECT_NO`, not on the full composite key.

By identifying these issues and decomposing the tables appropriately, redundancy is minimized, data consistency is maintained, and each design now follows proper normalization principles.
