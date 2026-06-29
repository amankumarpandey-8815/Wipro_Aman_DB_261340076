# Retrieving Data Using the SQL SELECT Statement 

## Aim

To understand the fundamental SQL **SELECT** statement and retrieve data from database tables using commands such as **DESC**, **SELECT**, and **WHERE**.

---

## Question 1 — Determine the Structure of the DEPT Table and its Contents.

### Explanation

The **DESC** command inspects the schema of a table, revealing its column names, data types, and constraints. The **SELECT** statement subsequently retrieves all records persisted in that table.

### SQL Query

#### Display the Structure of the DEPT Table
```sql
DESC DEPT;
```

#### Display the Contents of the DEPT Table
```sql
SELECT * FROM DEPT;
```

### Result

The `DESC` command outputs the complete schema of the **DEPT** table, including column definitions and applicable constraints. The `SELECT *` statement retrieves and displays every record currently stored within the table.

---

## Question 2 — Determine the Structure of the EMP Table and its Contents.

### Explanation

The **DESC** command exposes the column-level schema of the **EMP** table, while the **SELECT** statement fetches the complete set of employee records maintained within it.

### SQL Query

#### Display the Structure of the EMP Table
```sql
DESC EMP;
```

#### Display the Contents of the EMP Table
```sql
SELECT * FROM EMP;
```

### Result

The `DESC` command renders the full schema of the **EMP** table, detailing each column along with its associated data type. The `SELECT *` statement returns every employee record present in the table.

---

## Question 3 — Display the ENAME and DEPTNO from the EMP table whose EMPNO is 7788.

### Explanation

The **WHERE** clause filters result sets based on a defined condition, ensuring only records satisfying the specified criterion are returned. In this case, the query targets the employee uniquely identified by **EMPNO 7788**.

### SQL Query

```sql
SELECT ENAME, DEPTNO
FROM EMP
WHERE EMPNO = 7788;
```

### Result

The query returns the **Employee Name (ENAME)** and **Department Number (DEPTNO)** exclusively for the employee whose **EMPNO** is **7788**, demonstrating precise row-level filtering using the `WHERE` clause.

---

## Conclusion

This hands-on assignment establishes a foundational understanding of the SQL **SELECT** statement and its role in querying relational database tables.

| Command | Purpose |
|---|---|
| `DESC` | Displays the schema (columns, data types, constraints) of a table |
| `SELECT` | Retrieves records from one or more tables |
| `SELECT *` | Fetches all columns for every record in a table |
| `WHERE` | Filters records based on a specified condition |

These commands constitute the core building blocks of SQL and are indispensable for effective data retrieval in relational database management systems.
