# SQL Joins 

## Aim

To understand and perform different types of SQL Joins such as **Natural Join, Equi Join, Self Join, Outer Join, Non-Equi Join, USING Clause, and ON Clause** using the EMP, DEPT, and SALGRADE tables.

---

# Question 1

## Natural Join

### Question

Write a query for the HR department to produce the addresses of all the departments. Use the **EMP** and **DEPT** tables. Display **EMPNO, ENAME, SAL, DNAME, and LOC** using a **NATURAL JOIN**.

### Explanation

A **Natural Join** implicitly identifies and joins two tables on their shared column (`DEPTNO`), eliminating the need to explicitly define the join condition.

### SQL Query

```sql
SELECT EMPNO, ENAME, SAL, DNAME, LOC
FROM EMP
NATURAL JOIN DEPT;
```

### Result

The query returns each employee's details — including employee number, name, and salary — enriched with the corresponding department name and location derived from the **DEPT** table.

---

# Question 2

## Equi Join

### Question

Display the **JOB, MGR, SAL, COMM, and DNAME** of employees whose job is **SALESMAN**.

### Explanation

An **Equi Join** establishes a relationship between the **EMP** and **DEPT** tables by matching rows where the `DEPTNO` column holds equal values, with an additional filter applied to isolate employees holding the **SALESMAN** designation.

### SQL Query

```sql
SELECT E.JOB, E.MGR, E.SAL, E.COMM, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND E.JOB = 'SALESMAN';
```

### Result

The query retrieves compensation and managerial details for all employees designated as **SALESMAN**, alongside the name of the department to which each belongs.

---

# Question 3

## Equi Join

### Question

Display the **ENAME, JOB, DEPTNO, and DNAME** of employees who work in **DALLAS**.

### Explanation

The **EMP** and **DEPT** tables are joined on their common `DEPTNO` column, with a location-based filter applied to restrict the result set to departments whose office is situated in **Dallas**.

### SQL Query

```sql
SELECT E.ENAME, E.JOB, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND D.LOC = 'DALLAS';
```

### Result

The query returns the name, job title, department number, and department name of all employees assigned to departments physically located in **Dallas**.

---

# Question 4

## Self Join

### Question

Create a report to display employees' name and employee number along with their manager's name and manager number.

### Explanation

A **Self Join** references the **EMP** table twice under separate aliases, treating one instance as the employee record and the other as the manager record, since managerial relationships are stored within the same table.

### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.EMPNO AS "Emp#",
    M.ENAME AS Manager,
    M.EMPNO AS "Mgr#"
FROM EMP E
JOIN EMP M
ON E.MGR = M.EMPNO;
```

### Result

The query produces a paired report listing each employee's name and number alongside the corresponding reporting manager's name and number, as resolved from the same **EMP** table.

---

# Question 5

## Left Outer Join

### Question

Modify the previous query to display all employees including **KING**, who has no manager. Order the results by employee number.

### Explanation

A **LEFT OUTER JOIN** extends the self-join to include all employee records regardless of whether a matching manager entry exists, ensuring top-level employees such as **KING** — who carry a null `MGR` value — are retained in the result set.

### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.EMPNO AS "Emp#",
    M.ENAME AS Manager,
    M.EMPNO AS "Mgr#"
FROM EMP E
LEFT OUTER JOIN EMP M
ON E.MGR = M.EMPNO
ORDER BY E.EMPNO;
```

### Result

The query returns the complete employee roster ordered by employee number, with manager details populated where applicable and `NULL` displayed for employees who hold no managerial reporting relationship.

---

# Question 6

## Non-Equi Join

### Question

First display the structure of the **SALGRADE** table. Then display the employee name, job, department name, salary, and salary grade.

### SQL Query

```sql
DESC SALGRADE;
```

### Explanation

A **Non-Equi Join** correlates rows across tables using a range-based condition rather than an equality predicate, mapping each employee's salary to the appropriate grade band defined in the **SALGRADE** table.

### SQL Query

```sql
SELECT
    E.ENAME,
    E.JOB,
    D.DNAME,
    E.SAL,
    S.GRADE
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO
JOIN SALGRADE S
ON E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

### Result

The query returns each employee's name, job title, and department name from the **EMP** and **DEPT** tables, supplemented with a salary grade derived by matching the employee's salary against the applicable range in the **SALGRADE** table.

---

# Question 7

## Right Outer Join

### Question

Display the **ENAME** and **DNAME** of all employees. Also display department names that do not have any employees.

### Explanation

A **RIGHT OUTER JOIN** ensures that all records from the **DEPT** table are included in the result set, irrespective of whether any employee records in the **EMP** table correspond to a given department.

### SQL Query

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E
RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

### Result

The query returns all department names alongside their associated employee names, with `NULL` populated in the employee column for departments that currently have no assigned employees.

---

# Question 8

## Self Join with Outer Join

### Question

Find the names and hire dates for employees who were hired before their managers, along with their managers' names and hire dates.

### Explanation

The **EMP** table is self-joined under two aliases to compare the hire dates of employees against those of their respective managers, filtering only the records where the employee's tenure predates that of their manager.

### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.HIREDATE AS Employee_HireDate,
    M.ENAME AS Manager,
    M.HIREDATE AS Manager_HireDate
FROM EMP E
LEFT OUTER JOIN EMP M
ON E.MGR = M.EMPNO
WHERE M.HIREDATE > E.HIREDATE;
```

### Result

The query returns employee-manager pairs where the employee's hire date precedes the manager's, presenting both names and their respective hire dates for clear seniority comparison.

---

# Question 9

## USING Clause

### Question

Display the **EMPNO, ENAME, DNAME, and LOC** of employees working as **CLERK** using the **USING** clause.

### Explanation

The **USING** clause streamlines the join syntax by designating the shared column (`DEPTNO`) as the join key, avoiding column ambiguity and eliminating the need for table-qualified references to that column.

### SQL Query

```sql
SELECT EMPNO, ENAME, DNAME, LOC
FROM EMP
JOIN DEPT
USING (DEPTNO)
WHERE JOB = 'CLERK';
```

### Result

The query returns the employee number, name, department name, and office location for all employees holding the **CLERK** designation, with the join resolved cleanly through the shared `DEPTNO` column.

---

# Question 10

## ON Clause

### Question

Display the **ENAME, SAL, MGR, and DNAME** of employees whose salary is greater than **2000**.

### Explanation

The **ON** clause provides an explicit, unambiguous join condition between the **EMP** and **DEPT** tables, with a salary threshold filter subsequently applied to narrow the result set to higher-earning employees.

### SQL Query

```sql
SELECT E.ENAME, E.SAL, E.MGR, D.DNAME
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO
WHERE E.SAL > 2000;
```

### Result

The query retrieves the name, salary, manager reference, and department name for all employees whose compensation exceeds **2000**, demonstrating the use of the **ON** clause in conjunction with a conditional filter.

---

# Question 11

## LEFT OUTER JOIN

### Question

Display the **EMPNO, ENAME, JOB, DEPTNO, DNAME, and LOC** using **LEFT OUTER JOIN**.

### Explanation

A **LEFT OUTER JOIN** guarantees the inclusion of every record from the **EMP** table, appending matching department details from the **DEPT** table where available and substituting `NULL` where no corresponding department record exists.

### SQL Query

```sql
SELECT
    E.EMPNO,
    E.ENAME,
    E.JOB,
    E.DEPTNO,
    D.DNAME,
    D.LOC
FROM EMP E
LEFT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

### Result

The query returns the complete employee list with associated department name and location, preserving all employee records and displaying `NULL` for department attributes wherever no matching department entry is found.

---

# Question 12

## RIGHT OUTER JOIN

### Question

Display the **ENAME** and **DNAME** of employees using **RIGHT OUTER JOIN**.

### Explanation

A **RIGHT OUTER JOIN** prioritizes the **DEPT** table, ensuring every department record is present in the output regardless of whether any employee in the **EMP** table is assigned to that department.

### SQL Query

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E
RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

### Result

The query returns all department names with their associated employee names, retaining departments that have no current employee assignments and representing their employee column as `NULL`.

---

# Question 13

## FULL OUTER JOIN

### Question

Display the **EMPNO, DNAME, and LOC** using **FULL OUTER JOIN**.

### Explanation

A **FULL OUTER JOIN** produces a comprehensive result set by retaining all matched and unmatched records from both the **EMP** and **DEPT** tables, substituting `NULL` wherever a corresponding record is absent on either side.

### SQL Query

```sql
SELECT
    E.EMPNO,
    D.DNAME,
    D.LOC
FROM EMP E
FULL OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

### Result

The query returns a complete union of employee and department records, including unmatched entries from both tables — employees without a valid department and departments without any assigned employees — with `NULL` used to denote absent values on either side.

---

# Conclusion

This practical assignment demonstrates the implementation of different SQL Join operations.

| Join Type | Purpose |
|---|---|
| **Natural Join** | Joins tables automatically using commonly named columns |
| **Equi Join** | Matches rows based on equality of a shared column value |
| **Self Join** | Relates records within the same table using aliases |
| **Left Outer Join** | Returns all records from the left table, with NULLs for unmatched right records |
| **Right Outer Join** | Returns all records from the right table, with NULLs for unmatched left records |
| **Full Outer Join** | Returns all matched and unmatched records from both tables |
| **Non-Equi Join** | Joins records using a range-based condition instead of equality |
| **USING Clause** | Defines join key by column name, avoiding ambiguity |
| **ON Clause** | Specifies an explicit, fully qualified join condition |

These joins are essential for retrieving related data from multiple tables efficiently in relational database management systems.
