# Mini Project - Database Normalization (Practical)

## Aim

To understand and implement the concepts of **First Normal Form (1NF)**, **Second Normal Form (2NF)**, and **Third Normal Form (3NF)** by normalizing the given tables and writing the corresponding SQL queries.

---

# Question 1

## Normalize the Table into First Normal Form (1NF)

### Given Table

| Member_Id | First_Name | Last_Name | Hobbies |
|-----------|------------|-----------|---------|
| 101 | Jayson | Mark | Cricket, Swimming, Football |
| 102 | Ram | Ganesh | Swimming, Running, Music |
| 103 | Raj | Kishore | Dancing, Singing, Running |

### Current Issue

Looking at the **Hobbies** column, we can see that each cell is packed with more than one value, separated by commas. A single column is not supposed to carry multiple pieces of information at once — this breaks the fundamental rule of **First Normal Form (1NF)**, which states that every cell in a table must hold one and only one atomic value. Until this is fixed, the data cannot be queried or maintained reliably.

### Table after Normalization (1NF)

| Member_Id | First_Name | Last_Name | Hobby |
|-----------|------------|-----------|-------|
| 101 | Jayson | Mark | Cricket |
| 101 | Jayson | Mark | Swimming |
| 101 | Jayson | Mark | Football |
| 102 | Ram | Ganesh | Swimming |
| 102 | Ram | Ganesh | Running |
| 102 | Ram | Ganesh | Music |
| 103 | Raj | Kishore | Dancing |
| 103 | Raj | Kishore | Singing |
| 103 | Raj | Kishore | Running |

### SQL Query Used

```sql
CREATE TABLE Member_Hobbies (
    Member_Id  INT,
    First_Name VARCHAR(30),
    Last_Name  VARCHAR(30),
    Hobby      VARCHAR(30)
);

INSERT INTO Member_Hobbies VALUES
(101, 'Jayson', 'Mark',    'Cricket'),
(101, 'Jayson', 'Mark',    'Swimming'),
(101, 'Jayson', 'Mark',    'Football'),
(102, 'Ram',    'Ganesh',  'Swimming'),
(102, 'Ram',    'Ganesh',  'Running'),
(102, 'Ram',    'Ganesh',  'Music'),
(103, 'Raj',    'Kishore', 'Dancing'),
(103, 'Raj',    'Kishore', 'Singing'),
(103, 'Raj',    'Kishore', 'Running');

SELECT * FROM Member_Hobbies;
```

### Result

The table now satisfies **First Normal Form (1NF)** because every field stores only one value.

---

# Question 2

## Normalize the Table into Second Normal Form (2NF)

### Given Table

| EmpNo | Training | Training_Date | Dept |
|-------|----------|---------------|------|
| 101 | Oracle SQL | 12-Aug-2015 | TT |
| 101 | Java | 21-Aug-2015 | BU |
| 102 | Oracle SQL | 18-Sep-2014 | TT |

### Current Issue

Here the primary key is a combination of **EmpNo** and **Training** together. However, if we look closely at the **Dept** column, its value changes only when the employee changes — it has nothing to do with which training program is attended. In other words, **Dept** is tied to the employee alone, not to the training. This kind of situation, where a non-key column relies on just a part of the composite key, is called a **Partial Dependency** and it directly violates **Second Normal Form (2NF)**. To resolve this, the table needs to be split into two independent tables.

### Employee Table

| EmpNo | Dept |
|-------|------|
| 101 | TT |
| 102 | TT |

### Training Table

| EmpNo | Training | Training_Date |
|-------|----------|---------------|
| 101 | Oracle SQL | 12-Aug-2015 |
| 101 | Java | 21-Aug-2015 |
| 102 | Oracle SQL | 18-Sep-2014 |

### SQL Query Used

```sql
CREATE TABLE Employee (
    EmpNo INT PRIMARY KEY,
    Dept  VARCHAR(20)
);

CREATE TABLE Training (
    EmpNo         INT,
    Training      VARCHAR(50),
    Training_Date DATE,
    FOREIGN KEY (EmpNo) REFERENCES Employee(EmpNo)
);

INSERT INTO Employee VALUES
(101, 'TT'),
(102, 'TT');

INSERT INTO Training VALUES
(101, 'Oracle SQL', '2015-08-12'),
(101, 'Java',       '2015-08-21'),
(102, 'Oracle SQL', '2014-09-18');

SELECT * FROM Employee;
SELECT * FROM Training;
```

### Result

The partial dependency has been removed, and the tables now satisfy **Second Normal Form (2NF)**.

---

# Question 3

## Normalize the Table into Third Normal Form (3NF)

### Given Table

| Member_Id | First_Name | Last_Name | Sports | Fees |
|-----------|------------|-----------|--------|------|
| 101 | Rajesh | Chand | Cricket | 100 |
| 102 | Jayesh | Raj | Hockey | 80 |
| 103 | Mark | Dorsson | Football | 90 |

### Current Issue

At first glance this table may appear fine, but there is a hidden problem. The **Fees** column does not actually depend on the **Member_Id** — it depends on the **Sports** column instead. In other words, the fee is determined by the sport being played, not by who the member is. This creates an indirect chain of dependency: **Member_Id → Sports → Fees**, which is known as a **Transitive Dependency**. Such a dependency violates **Third Normal Form (3NF)** because non-key columns should only depend on the primary key, nothing else. The solution is to move the sports fee data into a separate table.

### Member Table

| Member_Id | First_Name | Last_Name | Sports |
|-----------|------------|-----------|--------|
| 101 | Rajesh | Chand | Cricket |
| 102 | Jayesh | Raj | Hockey |
| 103 | Mark | Dorsson | Football |

### Sports_Fee Table

| Sports | Fees |
|--------|------|
| Cricket | 100 |
| Hockey | 80 |
| Football | 90 |

### SQL Query Used

```sql
CREATE TABLE Member (
    Member_Id  INT PRIMARY KEY,
    First_Name VARCHAR(30),
    Last_Name  VARCHAR(30),
    Sports     VARCHAR(30)
);

CREATE TABLE Sports_Fee (
    Sports VARCHAR(30) PRIMARY KEY,
    Fees   INT
);

INSERT INTO Member VALUES
(101, 'Rajesh', 'Chand',   'Cricket'),
(102, 'Jayesh', 'Raj',     'Hockey'),
(103, 'Mark',   'Dorsson', 'Football');

INSERT INTO Sports_Fee VALUES
('Cricket',  100),
('Hockey',   80),
('Football', 90);

SELECT * FROM Member;
SELECT * FROM Sports_Fee;
```

### Result

The transitive dependency has been eliminated by separating sports fee details into a different table. The database now satisfies **Third Normal Form (3NF)**.

---

# Conclusion

This practical mini project demonstrates the process of database normalization.

- **1NF** removes repeating groups and multi-valued attributes.
- **2NF** removes partial dependencies by separating related data.
- **3NF** removes transitive dependencies by storing dependent information in separate tables.

Applying normalization improves data consistency, minimizes redundancy, and makes the database easier to maintain.
