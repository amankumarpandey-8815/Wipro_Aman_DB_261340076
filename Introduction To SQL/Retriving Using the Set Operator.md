# Retriving data Using the Set Operators 

## Aim

To understand and implement SQL **Set Operators** such as **UNION** and **UNION ALL** for combining the results of multiple SQL queries. This assignment also demonstrates the use of aggregate functions and matrix reports using the **EMPLOYEES** table.

---

## Question 1 — Create a matrix query to display the job, the salary for that job based on department number, and the total salary for that job, for departments 10, 20, and 30. Give each column an appropriate heading.

### Explanation

The **DECODE()** function is leveraged to pivot department-specific salary data into a matrix format, enabling a side-by-side comparison of salary totals across departments **10**, **20**, and **30**, along with a cumulative total for each job role.

### SQL Query

```sql
SELECT job,
       SUM(DECODE(department_id,10,salary)) AS Dept10,
       SUM(DECODE(department_id,20,salary)) AS Dept20,
       SUM(DECODE(department_id,30,salary)) AS Dept30,
       SUM(salary) AS Total
FROM employees
WHERE department_id IN (10,20,30)
GROUP BY job;
```

### Result

The query produces a structured matrix report presenting department-wise salary aggregates for each job role across departments 10, 20, and 30, accompanied by a grand total column reflecting the overall compensation per job.

---

## Question 2 — Using the SET operator, display the DEPTNO and total salary for each department, the JOB and total salary for each job, and the overall total salary.


### Explanation

The **UNION** operator merges the result sets of multiple **SELECT** statements, eliminating duplicate rows, to consolidate department-level salary totals, job-level salary totals, and the overall grand total into a single unified report.

### SQL Query

```sql
SELECT TO_CHAR(department_id) AS Department,
       NULL AS Job,
       SUM(salary) AS Total_Salary
FROM employees
GROUP BY department_id
UNION
SELECT NULL,
       job_id,
       SUM(salary)
FROM employees
GROUP BY job_id
UNION
SELECT 'TOTAL',
       NULL,
       SUM(salary)
FROM employees;
```

### Result

The query delivers a consolidated report encompassing department-wise salary aggregates, job-wise salary summaries, and an overall grand total, all presented within a single result set through the use of the **UNION** operator.

---

## Question 3 — Using the SET operator, display the JOB and DEPTNO of employees working in departments 20, 10, and 30 in the same order.

### Explanation

The **UNION ALL** operator concatenates the result sets of multiple **SELECT** statements while preserving duplicate records and maintaining the explicit ordering of each query block, ensuring departments are returned in the prescribed sequence of **20**, **10**, and **30**.

### SQL Query

```sql
SELECT job_id, department_id
FROM employees
WHERE department_id = 20
UNION ALL
SELECT job_id, department_id
FROM employees
WHERE department_id = 10
UNION ALL
SELECT job_id, department_id
FROM employees
WHERE department_id = 30;
```

### Result

The query returns the job identifiers and department numbers for all employees across departments **20**, **10**, and **30**, strictly preserving the specified departmental sequence and retaining all duplicate records as produced by **UNION ALL**.

---

## Conclusion

This hands-on assignment demonstrates the practical application of SQL **Set Operators** in constructing multi-dimensional reports and combining heterogeneous query results.

| Concept | Purpose |
|---|---|
| `UNION` | Merges result sets from multiple queries while eliminating duplicate rows |
| `UNION ALL` | Merges result sets while retaining all duplicate rows and preserving order |
| `DECODE()` | Enables conditional aggregation for constructing matrix-style pivot reports |
| `SUM()` | Computes aggregate salary totals at the department, job, and global level |

These SQL techniques are extensively employed in report generation and analytical querying within relational database management systems.
