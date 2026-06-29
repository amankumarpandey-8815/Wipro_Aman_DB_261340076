# Built-in Functions 

---

## Question 1

**Write a query to display the current date. Label the column Date.**

### Solution

```sql
SELECT SYSDATE AS "Date"
FROM dual;
```

---

## Question 2

**The HR department needs a report to display the employee number, last name, salary, and salary increased by 15.5% (expressed as a whole number) for each employee. Label the column New Salary.**

### Solution

```sql
SELECT employee_id,
       last_name,
       salary,
       ROUND(salary * 1.155) AS "New Salary"
FROM employees;
```

---

## Question 3

**Modify the previous query to add a column alias that subtracts the old salary from the new salary. Label the column Increase.**

### Solution

```sql
SELECT employee_id,
       last_name,
       salary,
       ROUND(salary * 1.155)          AS "New Salary",
       ROUND(salary * 1.155) - salary AS "Increase"
FROM employees;
```

---

## Question 4

**Write a query that displays the ename (with the first letter uppercase and all other letters lowercase) and the length of the ename for all employees whose name starts with the letters J, A, or M. Give each column an appropriate label. Sort the results by the employees' enames.**

### Solution

```sql
SELECT INITCAP(last_name) AS Name,
       LENGTH(last_name)  AS Length
FROM employees
WHERE UPPER(SUBSTR(last_name, 1, 1)) IN ('J', 'A', 'M')
ORDER BY last_name;
```

---

## Question 5

**Rewrite the query so that the user is prompted to enter a letter that starts the last name.**

### Solution

```sql
SELECT last_name
FROM employees
WHERE UPPER(last_name) LIKE UPPER('&letter') || '%';
```

---

## Question 6

**The HR department wants to find the length of employment for each employee.**

### Solution

```sql
SELECT last_name,
       MONTHS_BETWEEN(SYSDATE, hire_date) AS MONTHS_WORKED
FROM employees
ORDER BY MONTHS_WORKED;
```

---

## Question 7

**Create a report that produces the following for each employee: earns monthly but wants Salary × 3. Label the column Dream Salaries.**

### Solution

```sql
SELECT last_name || ' earns ' || salary ||
       ' monthly but wants ' || salary * 3 AS "Dream Salaries"
FROM employees;
```

---

## Question 8

**Create a query to display the last name and salary for all employees. Format the salary to be 15 characters long, left-padded with the $ symbol. Label the column SALARY.**

### Solution

```sql
SELECT last_name,
       LPAD(salary, 15, '$') AS SALARY
FROM employees;
```

---

## Question 9

**Display each employee's last name, hire date, and salary review date (first Monday after six months of service). Label the column REVIEW.**

### Solution

```sql
SELECT last_name,
       hire_date,
       NEXT_DAY(ADD_MONTHS(hire_date, 6), 'MONDAY') AS REVIEW
FROM employees;
```

---

## Question 10

**Display the last name, hire date, and day of the week on which the employee started. Label the column DAY.**

### Solution

```sql
SELECT last_name,
       hire_date,
       TO_CHAR(hire_date, 'Day') AS DAY
FROM employees
ORDER BY TO_CHAR(hire_date, 'D');
```

---

## Question 11

**Create a query that displays the employees' last names and commission amounts. If an employee does not earn commission, show "No Commission." Label the column COMM.**

### Solution

```sql
SELECT last_name,
       NVL(TO_CHAR(commission_pct), 'No Commission') AS COMM
FROM employees;
```

---

## Question 12

**Display the first eight characters of the employees' last names and indicate their salaries with asterisks.**

### Solution

```sql
SELECT RPAD(SUBSTR(last_name, 1, 8), 8, ' ') AS LAST_NAME,
       RPAD('*', TRUNC(salary / 1000), '*')   AS EMPLOYEES_AND_THEIR_SALARIES
FROM employees
ORDER BY salary DESC;
```

---

## Question 13

**Using the DECODE function, display the grade of all employees based on JOB_ID.**

### Solution

```sql
SELECT last_name,
       job_id,
       DECODE(job_id,
              'AD_PRES',  'A',
              'ST_MAN',   'B',
              'SA_REP',   'C',
              'ST_CLERK', 'D',
              '0') AS GRADE
FROM employees;
```

---

## Question 14

**Rewrite the previous statement using the CASE syntax.**

### Solution

```sql
SELECT last_name,
       job_id,
       CASE job_id
           WHEN 'AD_PRES'  THEN 'A'
           WHEN 'ST_MAN'   THEN 'B'
           WHEN 'SA_REP'   THEN 'C'
           WHEN 'ST_CLERK' THEN 'D'
           ELSE '0'
       END AS GRADE
FROM employees;
```

---

## Functions Reference Summary

| Function | Purpose | Example |
|----------|---------|---------|
| `SYSDATE` | Returns current date and time | `SELECT SYSDATE FROM dual` |
| `ROUND(n, d)` | Rounds a number to d decimal places | `ROUND(salary * 1.155)` |
| `INITCAP(str)` | Capitalizes the first letter of each word | `INITCAP(last_name)` |
| `LENGTH(str)` | Returns the length of a string | `LENGTH(last_name)` |
| `SUBSTR(str, pos, len)` | Extracts a substring | `SUBSTR(last_name, 1, 1)` |
| `UPPER(str)` | Converts string to uppercase | `UPPER(last_name)` |
| `MONTHS_BETWEEN(d1, d2)` | Number of months between two dates | `MONTHS_BETWEEN(SYSDATE, hire_date)` |
| `NEXT_DAY(date, day)` | Next occurrence of a weekday after a date | `NEXT_DAY(hire_date, 'MONDAY')` |
| `ADD_MONTHS(date, n)` | Adds n months to a date | `ADD_MONTHS(hire_date, 6)` |
| `TO_CHAR(date, fmt)` | Converts date to formatted string | `TO_CHAR(hire_date, 'Day')` |
| `LPAD(str, len, pad)` | Left-pads a string to a given length | `LPAD(salary, 15, '$')` |
| `RPAD(str, len, pad)` | Right-pads a string to a given length | `RPAD(last_name, 8, ' ')` |
| `TRUNC(n)` | Truncates a number (removes decimal) | `TRUNC(salary / 1000)` |
| `NVL(expr, default)` | Replaces NULL with a default value | `NVL(commission_pct, 'No Commission')` |
| `DECODE(expr, ...)` | Conditional value mapping (like IF-ELSE) | `DECODE(job_id, 'AD_PRES', 'A', ...)` |
| `CASE ... END` | Standard SQL conditional expression | `CASE job_id WHEN ... THEN ... END` |
