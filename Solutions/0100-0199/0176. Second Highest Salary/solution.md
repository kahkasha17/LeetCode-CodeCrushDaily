# [176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary)

## Description

<!-- description:start -->

<p>Table: <code>Employee</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.
</pre>

<p>&nbsp;</p>

<p>Write a solution to find&nbsp;the second highest <strong>distinct</strong> salary from the <code>Employee</code> table. If there is no second highest salary,&nbsp;return&nbsp;<code>null (return&nbsp;None in Pandas)</code>.</p>

<p>The result format is in the following example.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
<strong>Output:</strong> 
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
<strong>Output:</strong> 
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Use Sub Query and LIMIT

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    (
        SELECT DISTINCT salary
        FROM Employee
        ORDER BY salary DESC
        LIMIT 1, 1
    ) AS SecondHighestSalary;
```
### Solution 2: Use `MAX()` function

#### MySQL

```sql
# Write your MySQL query statement below
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```
### Solution 3: Use `IFNULL()` and window function

#### MySQL

```sql
# Write your MySQL query statement below
WITH T AS (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rk FROM Employee)
SELECT (SELECT DISTINCT salary FROM T WHERE rk = 2) AS SecondHighestSalary;
```
<!-- solution:end -->

<!-- problem:end -->