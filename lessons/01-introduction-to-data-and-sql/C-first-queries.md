---
title: "Write Your First Queries"
description: "Dive into the basics of SQL syntax and commands. Learn essential SQL components such as SELECT, FROM, WHERE, and how they form the building blocks for querying databases. Gain hands-on experience crafting simple SQL queries to retrieve specific data sets."
---

## Basics of SQL Queries

SELECT statements are the bedrock of SQL queries. This is us asking a database for some information back.

A typical query looks something like this:

```sql
SELECT column_name FROM table_name;
```

Five main parts that make up this statement

- SELECT
- column name
- FROM
- table name
- semi-colon

Quite often SELECT statement contains a list of columns that follow this keyword

The names of these columns come from a table that we are using for this query (‘our question’)

We add a comma after each column name, except for the last column

Then we need to include a FROM statement to specify the table that is required for this query

If we are working with a table called `person` and a want the `first_name` and `last_name` columns from it, we would do something like this:

```sql
SELECT 
first_name, 
last_name
FROM person;

```

But what if we just want everything from a table?

The * character is a wildcard that gets ALL columns from a table.  It also may be called ‘select list’ of all columns

```sql
SELECT *
FROM person;
```

Apparently this is bad practice??




Column names are qualified with the table name

```sql
SELECT 
person.first_name, 
person.last_name
FROM person;
```

Or we can use a table alias to save us writing out the table name before every column:

```sql
SELECT 
p.first_name, 
p.last_name
FROM person p;
```





FROM clause specifies the table that we want to query, i.e. the table that we want to use when asking a database a question. 

PLEASE NOTE

SQL syntax also allows us to qualify a column name with a table name – it is very useful when we write complex queries that involve multiple tables.

We can also alias a table name with any word or abbreviation – it helps us to avoid wordy statements and make our syntax neater. 






## Writing our first queries together

We're now going to write some of our first queries together.

In order for us all to be working on the exact same system, and with the same data, we are going to start by using an online tool called [Coderpad][coderpad]

<!-- TASK -->
First two queries:
- Write a query to show all project names and their budgets
- Show all information from the employees table



[coderpad]: https://coderpad.io/sandbox