# Reporting Aggregated Data Using Group Functions 

## Aim

To understand and implement SQL **Group Functions** such as **MAX(), MIN(), SUM(), AVG(), COUNT(), GROUP BY, HAVING, ORDER BY**, and aggregate calculations using the **EMPLOYEES** table.

---

# Question 1

## Find the highest, lowest, total, and average salary of all employees. Label the columns as Maximum, Minimum, Sum, and Average.

### Explanation

SQL aggregate functions condense multiple row values into a single computed result, enabling statistical analysis across an entire dataset. This query simultaneously applies **MAX()**, **MIN()**, **SUM()**, and **AVG()** to derive a comprehensive salary summary for the full employee population.

### SQL Query

```sql
SELECT MAX(salary) AS Maximum,
       MIN(salary) AS Minimum,
       SUM(salary) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees;
```

### Result

The query returns a single-row summary presenting the highest, lowest, cumulative, and mean salary values across all records in the **EMPLOYEES** table.

---

# Question 2

## Round the results to the nearest whole number.

### Explanation

The **ROUND()** function is applied as a wrapper around each aggregate expression to eliminate decimal precision from the output, ensuring all salary figures are presented as whole numbers.

### SQL Query

```sql
SELECT ROUND(MAX(salary)) AS Maximum,
       ROUND(MIN(salary)) AS Minimum,
       ROUND(SUM(salary)) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees;
```

### Result

The query produces the same four salary aggregates as the preceding query, with each value rounded to its nearest integer for cleaner, more readable output.

---

# Question 3

## Display the minimum, maximum, total, and average salary for each job type.

### Explanation

The **GROUP BY** clause partitions the **EMPLOYEES** table into distinct subsets by `job_id`, allowing the aggregate functions to independently compute salary statistics for each job category rather than across the entire table.

### SQL Query

```sql
SELECT job_id,
       MIN(salary) AS Minimum,
       MAX(salary) AS Maximum,
       SUM(salary) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees
GROUP BY job_id;
```

### Result

The query returns one row per job type, each containing the minimum, maximum, total, and average salary computed exclusively for employees holding that designation.

---

# Question 4

## Display the number of employees having the same job.

### Explanation

The **COUNT()** function is applied in conjunction with **GROUP BY** to tally the number of employee records associated with each distinct job category, producing a frequency distribution across all job types.

### SQL Query

```sql
SELECT job_id,
       COUNT(*) AS Number_of_People
FROM employees
GROUP BY job_id;
```

### Result

The query returns each unique job identifier alongside the total count of employees currently assigned to that role, reflecting the workforce distribution across all job categories.

---

# Question 5

## Determine the number of managers without listing them. Label the column as Number of Managers.

### Explanation

**COUNT(DISTINCT)** is employed to enumerate only unique `manager_id` values within the **EMPLOYEES** table, effectively counting each manager once regardless of how many subordinates they oversee.

### SQL Query

```sql
SELECT COUNT(DISTINCT manager_id) AS "Number of Managers"
FROM employees;
```

### Result

The query returns a single scalar value representing the total count of distinct managers across the organization, without exposing any individual manager's identity.

---

# Question 6

## Find the difference between the highest and lowest salaries. Label the column as DIFFERENCE.

### Explanation

The salary spread is computed by subtracting the result of **MIN(salary)** from **MAX(salary)** within a single expression, quantifying the full compensation range across the employee population.

### SQL Query

```sql
SELECT MAX(salary) - MIN(salary) AS DIFFERENCE
FROM employees;
```

### Result

The query returns a single value representing the absolute salary gap between the highest-compensated and lowest-compensated employees in the **EMPLOYEES** table.

---

# Question 7

## Display the manager number and the salary of the lowest-paid employee for each manager. Exclude employees without managers and groups whose minimum salary is 2000 or less. Sort the result in descending order of salary.

### Explanation

The query groups employee records by `manager_id`, applies **MIN()** to identify the lowest salary within each group, uses the **HAVING** clause to exclude groups where that minimum falls at or below **2000**, and sorts the qualifying results in descending salary order.

### SQL Query

```sql
SELECT manager_id,
       MIN(salary) AS Lowest_Salary
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING MIN(salary) > 2000
ORDER BY Lowest_Salary DESC;
```

### Result

The query returns manager IDs alongside the salary of their lowest-paid subordinate, restricted to managers whose entire team earns above **2000**, and ordered from the highest to lowest minimum salary.

---

# Question 8

## Display the total number of employees and the number of employees hired in 1980, 1981, and 1982.

### Explanation

Conditional aggregation is implemented using **CASE** expressions nested within **SUM()**, assigning a value of **1** to each employee record matching a target hire year and **0** otherwise, thereby producing a year-wise headcount alongside the overall total within a single query pass.

### SQL Query

```sql
SELECT COUNT(*) AS Total_Employees,
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1980 THEN 1 ELSE 0 END) AS "1980",
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1981 THEN 1 ELSE 0 END) AS "1981",
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1982 THEN 1 ELSE 0 END) AS "1982"
FROM employees;
```

### Result

The query delivers a single-row summary presenting the total employee count alongside individual hiring tallies for **1980**, **1981**, and **1982**, enabling year-on-year recruitment comparison in a compact tabular format.

---

# Conclusion

This hands-on assignment demonstrates the use of SQL **Aggregate Functions** for analyzing and summarizing data.

| Function / Clause | Purpose |
|---|---|
| `MAX()` | Returns the highest value within a column |
| `MIN()` | Returns the lowest value within a column |
| `SUM()` | Computes the cumulative total of a numeric column |
| `AVG()` | Calculates the arithmetic mean of a numeric column |
| `COUNT()` | Counts the number of rows or distinct values |
| `GROUP BY` | Partitions rows into subsets for per-group aggregation |
| `HAVING` | Filters aggregated groups based on a specified condition |
| `ORDER BY` | Sorts the final result set in ascending or descending order |

These functions are essential for generating reports and performing data analysis in SQL databases.
