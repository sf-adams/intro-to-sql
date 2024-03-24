---
title: "Logical Operators"
description: "Description"
---

So far we have looked at querying our data to return all entries in specific columns:

```sql
SELECT (column_1, column_2) FROM table_name;
```

However, often we don't want all information to be returned. We might be looking for a specific value, certain values to be left out or a number of other conditions. For that we introduce comparison and logical operators:

1. **Comparison Operators:** These operators are used to compare values. Common examples include:

- **=** : Equal to
- **!=** or **<>** : Not equal to
- **<**: Less than
- **\>** : Greater than
- **<=** : Less than or equal to
- **\>=** : Greater than or equal to

Examples using comparison operators:

```sql
-- Selects all employees whose age is greater than 30
SELECT * FROM employees WHERE age > 30;

-- Selects all products with a price less than or equal to 100
SELECT * FROM products WHERE price <= 100;

-- Selects all customers whose name is not 'John'
SELECT * FROM customers WHERE name != 'John';

```

2. **Logical Operators:** These operators are used to combine multiple conditions. Common examples of these are:

- **AND:** Returns true if both conditions are true.
- **OR:** Returns true if at least one condition is true.
- **NOT:** Negates the condition, returns true if the condition is false.

```sql
-- Selects all employees whose age is greater than 30
-- and have a salary greater than 50000
SELECT * FROM employees WHERE age > 30 AND salary > 50000;

-- Selects all orders with a quantity greater than 10
--or with a total price greater than 1000
SELECT * FROM orders WHERE quantity > 10 OR total_price > 1000;

-- Selects all products that are not out of stock
SELECT * FROM products WHERE NOT stock_quantity = 0;

-- Select all products that don't have an empty supplier
-- (you could also do IS NULL if you wanted to fill in suppliers)
SELECT p.name FROM products p WHERE supplier IS NOT NULL;

```

As you can see from these examples, we have introduced the `WHERE` keyword to specify a condition or multiple conditions that the rows must meet in order to be included in the result set of a query. From our previous queries we know that this keyword is optional in our `SELECT` statement, but will commonly be used to filter rows based on specific conditions.

We can also use parentheses to split the different sections:

```sql
SELECT g.home_score, g.away_score FROM game AS g
WHERE (fk_home_team_id = 8 OR fk_away_team_id = 8)
AND home_score > 30;
```
