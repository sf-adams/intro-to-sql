---
title: "Database Design and Management"
description: "Learning to organise our tables and plan how we are going to set out our relations."
---

## Normalisation

We normalise databases for a number of reasons:

- minimising duplicated data
- minimise or avoid data modification issues
- to simplify queries

There are three main levels of normalisation that we use: 1st, 2nd or 3rd normal form. It is an incremental scale, so each level we add further rules to make it more specific.

<!-- Need more info on these three to make sure it is extremely clear -->

### First Normal Form

There are two key rules to remember when thinking about applying 1NF:

1. Each attribute should contain only one value (e.g. wouldn't have BANANA and APPLE in the same space, we would have a separate table for fruit, and then the primary key from our first table would be used to identify who has which fruit.)
2. All attribute values are atomic, which means they can't be broken down into anything smaller (so full name would be split down into first_name and last_name)

### Second Normal Form

In order for a relation to be 2NF, it first has to pass the 1NF level.

This form requires us to `remove partial dependencies`,

### Third Normal Form

so if a column doesn't have a clear link to the `PRIMARY KEY` we move it to another table and set up a relation between them.

<!-- Review the db design in the sandbox: https://coderpad.io/sandbox -->

## Constraints

These are rules that we can apply on the type of data in a table. They ensure accuracy and reliability in our table columns. If the data we are trying to enter does not meet the requirements of the constraint, then the action will be aborted.

Beyond just choosing the value of our tables and what type of data, we have a number of other terms we can use:

- _NOT NULL:_ A value has to be entered when an entry is added. It can not be left blank.

<!-- Check that NOT NULL can be applied later to a table? Or does it have to be during the CREATE TABLE part? -->

- _UNIQUE:_ All values in this column must be different.

- _CHECK:_

- _PRIMARY KEY:_ One or more unique fields that will serve as the definition of a record. They have to be unique, can't be null, can be multiple columns (a compound key). They also don't have to be defined at the `CREATE TABLE` period, they can also be edited later with the `ALTER TABLE` statement.

<!-- Does the foreign key have to be linked to a primary key? Or is it just another unique field in a table? -->

- _FOREIGN KEY:_ A link to the primary key of another table. So for example, the id of a `user` may be the primary key in the `user` table, but could be the `FOREIGN KEY` when used elsewhere. A table can have lots of foreign keys, and they can all relate to different tables.
  (Have a read: http://www.mysqltutorial.org/mysql-foreign-key/)

- _DEFAULT:_ We can set a default value if no other value is entered

Some examples of constraints:

<!-- Should this be using integer? Or should it be an INT? Didn't know you could write the full? -->

```sql
CREATE TABLE customers
(
  customer_id INTEGER PRIMARY KEY,
  name VARCHAR(50),
  surname VARCHAR(50) NOT NULL,
  telephone INTEGER
);
```

Here's another alternative where we can declare the constraints later:

```sql
CREATE TABLE customers
(customer_id INTEGER,
 name VARCHAR(50),
 surname VARCHAR(50) NOT NULL,
 telephone INTEGER,
CONSTRAINT
pk_ customer _id
PRIMARY KEY
(customer _id)
);
```

> Quick note on using an index in SQL. https://www.mysqltutorial.org/mysql-index/ Making it quicker for SQL to identify rows by using the unique value. This is quicker than querying non-key or non-indexed values, as otherwise SQL has to work distinctly harder

## Relationships

So far we have talked a little bit about using separate tables for different types of entities.

However, we also have to take into account the relationships between these different tables:

<!-- Properly work out the difference in these, especially those middle two -->
- One to One Relationships
- One to Many
- Many to One Relationships
- Many to Many Relationships

For instance, customers may have many orders, but an order will only have one customer. 