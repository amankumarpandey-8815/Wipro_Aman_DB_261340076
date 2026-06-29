# Restricting and Sorting Data

## Aim

To understand and implement SQL statements for **restricting**, **sorting**, **filtering**, and **formatting** data using clauses such as **WHERE**, **ORDER BY**, **DISTINCT**, **BETWEEN**, **IN**, **LIKE**, **IS NULL**, and **column aliases** on the **EMP** table.

---

## Question 1

**Display the ENAME, SAL, and COMM of employees who earn commission. Sort the records in descending order of Salary and Commission using the column's numeric position in the ORDER BY clause.**

**Explanation**
The `WHERE COMM IS NOT NULL` clause restricts the result set to only those employees who have been assigned a commission value. The `ORDER BY 2 DESC, 3 DESC` clause sorts the output using numeric column positions, ordering first by salary and then by commission in descending order.

```sql
SELECT ENAME, SAL, COMM
FROM EMP
WHERE COMM IS NOT NULL
ORDER BY 2 DESC, 3 DESC;
```

**Result**
The query returns the names, salaries, and commission values of all commission-earning employees, arranged from the highest to the lowest salary, with ties further resolved by commission amount.

---

## Question 2

**Display all unique job codes from the EMP table.**

**Explanation**
The `DISTINCT` keyword eliminates duplicate entries from the result set, ensuring that each job title appears only once regardless of how many employees share that designation.

```sql
SELECT DISTINCT JOB
FROM EMP;
```

**Result**
The query returns a consolidated list of all unique job titles present in the EMP table, with no repetitions.

---

## Question 3

**Display employee number, employee name, job, and hire date with column aliases.**

**Explanation**
Column aliases are assigned using the `AS` keyword to replace default column headers with descriptive, user-friendly labels, improving the readability of the output report.

```sql
SELECT EMPNO   AS "Emp #",
       ENAME   AS "Employee",
       JOB     AS "Job",
       HIREDATE AS "Hire Date"
FROM EMP;
```

**Result**
The query returns all employee records with column headings displayed as *Emp #*, *Employee*, *Job*, and *Hire Date* instead of the default database column names.

---

## Question 4

**Display the employee name concatenated with the job title. Name the column "Employee and Title".**

**Explanation**
The concatenation operator (`||`) merges the `ENAME` and `JOB` columns into a single output string, with a comma and space inserted between them for proper formatting.

```sql
SELECT ENAME || ', ' || JOB AS "Employee and Title"
FROM EMP;
```

**Result**
The query returns a single column displaying each employee's name followed by their job title, formatted as `EMPLOYEE NAME, JOB TITLE`.

---

## Question 5

**Display ENAME, JOB, HIREDATE, and MGR in a single output column separated by commas. Name the column THE_OUTPUT.**

**Explanation**
Multiple column values are merged into one unified output string using the concatenation operator (`||`), with comma separators placed between each field to maintain a structured, readable format.

```sql
SELECT ENAME || ',' || JOB || ',' || HIREDATE || ',' || MGR AS THE_OUTPUT
FROM EMP;
```

**Result**
The query produces a single column, `THE_OUTPUT`, containing each employee's name, job title, hire date, and manager ID concatenated into one formatted string per record.

---

## Question 6

**Display the ENAME, JOB, and HIREDATE of employees named SCOTT or TURNER. Sort the records by hire date in ascending order.**

**Explanation**
The `IN` operator efficiently filters rows where the employee name matches any value in the specified list, and `ORDER BY HIREDATE ASC` arranges the results chronologically from earliest to latest.

```sql
SELECT ENAME, JOB, HIREDATE
FROM EMP
WHERE ENAME IN ('SCOTT', 'TURNER')
ORDER BY HIREDATE ASC;
```

**Result**
The query returns the job details and hire dates of employees SCOTT and TURNER, ordered by their date of joining in ascending chronological order.

---

## Question 7

**Display the ENAME and DEPTNO of employees working in departments 20 or 30. Sort the records alphabetically by employee name.**

**Explanation**
The `IN` clause filters records belonging to departments 20 and 30, while `ORDER BY ENAME` sorts the resulting employee list in alphabetical order for easier reference.

```sql
SELECT ENAME, DEPTNO
FROM EMP
WHERE DEPTNO IN (20, 30)
ORDER BY ENAME;
```

**Result**
The query returns the names and department numbers of all employees assigned to departments 20 or 30, listed in ascending alphabetical order by employee name.

---

## Question 8

**Display the employee name and salary of employees earning between 2000 and 3000 and working in departments 20 or 30. Use the aliases Employee and Monthly Salary.**

**Explanation**
The `BETWEEN` operator defines an inclusive salary range, and the `IN` clause restricts the result to specific departments. Column aliases are applied to present the output with meaningful headings.

```sql
SELECT ENAME AS "Employee",
       SAL   AS "Monthly Salary"
FROM EMP
WHERE SAL BETWEEN 2000 AND 3000
  AND DEPTNO IN (20, 30);
```

**Result**
The query returns employees from departments 20 or 30 whose monthly salary falls within the range of 2,000 to 3,000, with column headers displayed as *Employee* and *Monthly Salary*.

---

## Question 9

**Display the employee name and hire date of all employees hired in 1981.**

**Explanation**
The `BETWEEN` operator is applied to the `HIREDATE` column to filter all employees whose joining date falls within the calendar year 1981, from January 1st through December 31st.

```sql
SELECT ENAME, HIREDATE
FROM EMP
WHERE HIREDATE BETWEEN '01-JAN-81' AND '31-DEC-81';
```

**Result**
The query returns the names and hire dates of all employees who joined the organization during the year 1981.

---

## Question 10

**Display the ENAME and SAL of employees whose salary is greater than a user-specified value.**

**Explanation**
The `ACCEPT` command prompts the user to provide a salary threshold at runtime, which is then referenced in the `WHERE` clause using the substitution variable `&AMT` to dynamically filter the result set.

```sql
ACCEPT AMT PROMPT 'Enter Salary : '

SELECT ENAME, SAL
FROM EMP
WHERE SAL > &AMT;
```

**Result**
The query accepts a salary value from the user at runtime and returns the names and salaries of all employees earning more than the entered amount.

---

## Question 11

**Display the employee name and job title of employees who do not have a manager.**

**Explanation**
The `IS NULL` condition identifies records where the `MGR` column contains no value, indicating that the employee does not report to any manager — typically the top-level executive in the hierarchy.

```sql
SELECT ENAME, JOB
FROM EMP
WHERE MGR IS NULL;
```

**Result**
The query returns the name and job title of employees with no assigned manager, representing the highest level of the organizational hierarchy.

---

## Question 12

**Prompt the user for a Manager ID and display EMPNO, ENAME, SAL, and DEPTNO. Allow the user to sort the result by any selected column.**

**Explanation**
Two `ACCEPT` commands collect user input at runtime — one for the manager ID used in the `WHERE` clause filter, and another for the column name used as the dynamic sort key in the `ORDER BY` clause.

```sql
ACCEPT MID PROMPT 'Enter Manager ID : '
ACCEPT COL PROMPT 'Enter Column Name : '

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE MGR = &MID
ORDER BY &COL;
```

**Result**
The query returns employee records reporting to the specified manager and displays them in the order of the column selected by the user at runtime.

---

## Question 13

**Prompt the user for a Manager ID and generate EMPNO, ENAME, SAL, and DEPTNO for employees working under that manager. Allow sorting by a selected column.**

**Explanation**
This query follows the same dynamic input approach as Question 12, using substitution variables to accept a manager ID and a sort column from the user, enabling flexible and interactive query execution.

```sql
ACCEPT MID PROMPT 'Enter Manager ID : '
ACCEPT COL PROMPT 'Enter Column Name : '

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE MGR = &MID
ORDER BY &COL;
```

**Result**
The query retrieves all employees under the specified manager and presents the results sorted according to the column name entered by the user during execution.

---

## Question 14

**Display all employee names in which the third letter is 'A'.**

**Explanation**
The `LIKE` operator is used with two underscore wildcards (`__`) to skip the first two characters and match the letter `A` at exactly the third position, followed by `%` to allow any remaining characters.

```sql
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '__A%';
```

**Result**
The query returns the names of all employees whose third character is the letter `A`, regardless of the length or content of the remaining characters in the name.

---

## Question 15

**Display the names of employees who have both 'A' and 'S' in their names.**

**Explanation**
Two separate `LIKE` conditions are combined using the `AND` operator — each using the `%` wildcard — to ensure that both the letter `A` and the letter `S` are present anywhere within the employee name.

```sql
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '%A%'
  AND ENAME LIKE '%S%';
```

**Result**
The query returns the names of all employees whose names contain both the letters `A` and `S`, irrespective of their positions within the name.

---

## Question 16

**Display the ENAME, JOB, and SAL of employees whose job is CLERK and whose salary is either 800, 950, or 1300.**

**Explanation**
The `JOB = 'CLERK'` condition restricts the result to clerical staff, while the `IN` operator checks whether the salary matches any one of the three specified values, replacing multiple `OR` conditions with a cleaner syntax.

```sql
SELECT ENAME, JOB, SAL
FROM EMP
WHERE JOB = 'CLERK'
  AND SAL IN (800, 950, 1300);
```

**Result**
The query returns the name, job title, and salary of all clerks whose salary is exactly 800, 950, or 1,300 — filtering out clerks with any other salary value.

---

## Conclusion

This hands-on assignment demonstrates the practical use of essential SQL clauses for restricting, sorting, and formatting data retrieved from the EMP table.

| Clause / Feature | Purpose |
|---|---|
| `WHERE` | Filters records based on one or more conditions |
| `ORDER BY` | Sorts query results in ascending or descending order |
| `DISTINCT` | Eliminates duplicate values from the result set |
| `BETWEEN` | Filters values within a specified inclusive range |
| `IN` | Matches a column value against a list of specified values |
| `LIKE` | Performs pattern-based matching using wildcard characters |
| `IS NULL` | Identifies records where a column contains no value |
| Column Aliases | Assigns descriptive labels to columns for improved readability |
| `ACCEPT` | Accepts user input at runtime for dynamic query execution |

Mastery of these SQL features is fundamental to writing precise, efficient, and readable queries for data retrieval and reporting in relational database systems.
