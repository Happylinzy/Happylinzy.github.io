---
title: SQL Notes
date: 2023-01-29 20:36:57
categories: SQL
tags: Programing Language
---
This file records SQL usage I encountered(mainly from LeetCode).
Notation outside of code block: keywords in SQL will be in **bold**, variables will be in *Italic*.

<!-- more -->

### SELECT

**SELECT** basic structure:

```SQL
SELECT something
From some_table
Where requirements
```

Here *requirements* support keyword in *sometable* but not specified in *something*.

Example: 

Write an SQL query to find the ids of products that are both low fat and recyclable
```SQL
select product_id
from Products
where low_fats = 'Y' and recyclable = 'Y'
```

If we want to rename *something* in our result, we can use **as** keyword.
```SQL
select name as Customers
from Customers
where id not in 
(
    select customerId
    from Orders
)
```

### Union

**Union** will allow us to pile two tables with the same colomn together.

Example:

Write an SQL query to calculate the bonus of each employee. The bonus of an employee is 100% of their salary if the ID of the employee is an odd number and the employee name does not start with the character 'M'. The bonus of an employee is 0 otherwise.

```SQL
(select employee_id , salary as bonus 
from employees 
where employee_id%2 <>0 and name not like 'M%'
)
union
(
select employee_id , 0 as bonus
from employees
where employee_id%2 = 0 or name like 'M%'
)
order by employee_id;
```
Inside this query, we use **order by**, which will sort the result based on *employee_id*.

### Update

**UPDATE** is used to update entities inside a table.

```SQL
UPDATE salary
SET
    sex = CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
```
In this example, **CASE** is used to update the value of *sex* bases on its original value.

### DELETE

```SQL
DELETE p1 FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id > p2.Id
```
This example shows how we can construct an inner comparision requirement within one query. Also it shows how to declear variable *p1, p2* inside a query.

The above query can also be achieved by using **GROUP BY** keyword.
```SQL
DELETE FROM Person WHERE Id NOT IN 
(SELECT * FROM(SELECT MIN(Id) FROM Person GROUP BY Email) as p);
```

### GROUP_CONCAT

**GROUP_CONCAT** is an aggregate_function used to concat all items with in the same group specified in **group by**.
**DISTINCT** is used to delete repeated entities given a colomn number.

```SQL
select sell_date, COUNT( DISTINCT product) as num_sold, GROUP_CONCAT(DISTINCT product ORDER BY product) as products
from activities
group by sell_date
order by sell_date
```

### LIKE
**LIKE** is used to match strings. It supports '%' or '_' etc.
```SQL
select patient_id, patient_name, conditions
from Patients
where conditions like 'DIAB1%' or conditions like '% DIAB1%'
```