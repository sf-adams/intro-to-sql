---
title: "Database Design Fundamentals"
description: "In this section, we delve into the foundational concepts of database design, focusing on key principles such as normalization to eliminate redundancy and ensure data integrity, constraints to enforce rules and maintain data consistency, relationships to establish connections between entities, and indexes to optimize query performance. Through comprehensive exploration and practical examples, participants will gain a deep understanding of how to structure databases effectively to meet business requirements while adhering to best practices in database design."
---

## Normalisation

The process of organising data in a database efficiently. We normalise databases for a number of reasons:

- **Minimise data redundancy & duplication** - we organise our data into separate tables and linking them through relationships. This ensures that each piece of information is stored in only one place, and reduces the risk of inconsistencies.
- **Prevent data anomalies** - because tables are broken down with relations, less data needs to be inserted, so fewer errors. The same is true when we are modifying - if I have to modify or delete the same data in multiple places, there is a risk of error. If I have just one point, and then links to other tables, then it is easier to manage.
- **Improve data consistency and integrity** - it helps to maintain data consistency and integrity by enforcing rules that ensure each piece of data is stored and structured in a controlled manner.
- **Enhanced query performance** - by reducing redundant data, normalisation can lead to smaller, more efficient tables, making queries faster and more responsive.

There are three main levels of normalisation that we use: 1st, 2nd or 3rd normal form. It is an incremental scale, so each level we add further rules to make it more specific.

1. **First Normal Form (1NF)** - In 1NF, _each column in a table contains atomic (indivisible) values_ e.g. "Jane Doe" could be split into "Jane" and "Doe". There also can't be _repeating groups or arrays of values_ e.g. "Oranges, Bananas" will likely need separate entries in a new table, where they link to a primary key to work out who has which fruit.
2. **Second Normal Form (2NF)** - Has to be in 1NF, and _all non-key attributes have to be fully functionally dependent on the entire primary key_. We typically decompose tables into smaller, related tables where each non-key attribute then depends on the entire primary key. This is typically more useful for tables where there is a composite key.
3. **Third Normal Form (3NF)** - Has to be in 2NF, and eliminates _transitive dependencies_ by ensuring that _each non-key attribute is directly dependent on the primary key_. To achieve it, we would further decompose tables to isolate attributes that are functionally dependent on other non-key attributes.

> Review the db design in the [Coderpad Sandbox][sandbox]. You will probably see that it appears to be normalised up to at least 1NF, as each column holds atomic values and there are no repeating groups. However, to establish 2NF and 3NF we need to analyse the partial and transitive dependencies among attributes within each table.

## Constraints

These are rules that we can apply to `columns` or `tables` to limit the data that can be entered. They ensure accuracy and reliability in our table columns. If the data we are trying to enter does not meet the requirements of the constraint, then the action will be aborted.

Beyond just choosing the value of our tables and what type of data, we have a number of other terms we can use:

- **NOT NULL:** A value has to be entered when an entry is added. It can not be left blank. This constraint _can't be defined at table level_.

- **UNIQUE:** All values in this column must be different, no duplicate values are allowed. It is slightly different to the `PRIMARY KEY` constraint, in that it allows `NULL` values. (There are some exceptions with composite unique constraints, but that's outwith the scope of this lesson) To use this constraint:

- **CHECK:** Allows you to define a condition that must be satisfied for each row in a table.

- **PRIMARY KEY:** One or more unique fields that will serve as the definition of a record. They have to be unique, not NULL, can be multiple columns (a compound key). It can be defined in the `CREATE TABLE` query:

```sql
-- Directly after the column definition
CREATE TABLE table_name (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    ...
);

-- At the end of the CREATE TABLE statement, adding a composite key
CREATE TABLE table_name (
    id INT,
    name VARCHAR(50),
    ...
    PRIMARY KEY (id, name)
);

```

They can also be added later with the `ALTER TABLE` statement:

```sql
-- You can give a name to the Primary Key
ALTER TABLE table_name
ADD CONSTRAINT pk_table_name_id PRIMARY KEY (id);

-- Shorthand syntax, which will automatically assign pk_table_name_id
ALTER TABLE table_name
ADD PRIMARY KEY (id);

```

> In MySQL, if a PRIMARY KEY is not defined as NOT NULL, [MySQL declares them so implicitly and silently][mysql-primary].

- **FOREIGN KEY:** A column or set of columns in a table that establishes a link between data in two tables. It represents a relationship between the table containing the foreign key (child table) and the table being referenced (parent table). A table can have lots of foreign keys, and they can all relate to different tables.

- **DEFAULT:** We can set a default value if no other value is entered

Here is an example tying in all these together:

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    department_id INT,
    salary DECIMAL(10, 2) DEFAULT 0.0,
    CONSTRAINT fk_department_id FOREIGN KEY (department_id) REFERENCES departments (department_id)
);
```

There are some constraints we can add later with `ADD CONSTRAINT`:

```sql
ALTER TABLE table_name
ADD CONSTRAINT CHECK | PRIMARY KEY | FOREIGN KEY | UNIQUE (column_name);
```

And there are others we add with `MODIFY COLUMN`:

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name data_type NOT NULL | DEFAULT default_value;
```

> Please note that if you try to add this constraint to a table with data in it, you may run into issues. Your DBMS may check the existing data to make sure that it meets the new constraint. If a row violates the constraint, the DBMS might raise and error and the ALTER TABLE statement will fail.

## Relationships

So far we have talked a little bit about using separate tables for different types of entities.

However, we also have to take into account the relationships between these different tables:

- **One-to-One Relationship (1:1):** Each record in one table is related to exactly one record in another table. This is a very rare type of relationship, as it's often more efficient to combine the related data into a single table. E.g. a table called _person_ may have a one-to-one relationship with a _passport_ table, where each person has exactly one passport. In SQL, you would have to make sure that the `FOREIGN KEY` for person is a `UNIQUE` value, ensuring each passport can be associated with at most one person.
- **One-to-Many Relationship (1:M):** Each record in one table can be related to one or more records in another table, but each record in the second table is related to at most one record in the first table. _This is the most common type of relationship in a relational database._ E.g. a _customer_ table may have a one-to-many relationship with an _order_ table, where each customer can place multiple orders, but each order is placed by exactly one customer.
- **Many-to-One Relationship (M:1):** The inverse of a one-to-many relationship. It occurs when multiple records in one table are related to exactly one record in another table. E.g. with the _customer_ and _order_ tables in the previous example, the _order_ table is on the many side, but can only have one _customer_.
- **Many-to-Many Relationship (M:N):** This is best demonstrated with an example. Consider a university database. Students can enroll in multiple courses, and each course can have multiple students enrolled. This relationship is many-to-many because each student can be associated with multiple courses, and each course can have multiple students enrolled. To implement this, you typically create three tables: _student_, _course_ and a junction table called _enrollment_ which maps the relationships between students and courses. The junction table contains foreign keys referencing the primary keys of both the _student_ and _course_ tables.

For instance, customers may have many orders, but an order will only have one customer.

## Indexes

These are database objects that improve the speed of data retrieval operations on tables. MySQL uses indexes to rapidly locate rows with specific column values. Without an index, MySQL must scan the entire table to find the relevant rows. As tables grow larger, the slower the search becomes.

When we create an index on a table, our database system builds and maintains a separate data structure that stores the indexed column(s) sorted in a particular order.

There are several key types of indexes:

- **Single-Column Index:** An index created on a single column.
- **Composite Index:** An index created on multiple columns, allowing for quicker retrieval based on combinations of values in those columns.
- **Unique Index:** An index that enforces uniqueness of values in one or more columns, preventing duplicate entries.
- **Primary Key Index:** The primary key constraint automatically creates an index on the primary key column(s) to enforce uniqueness and optimize data retrieval. MySQL automatically creates a special index named `PRIMARY`, which is a version of a `Clustered Index`. It is stored together with the data in the same table, enforcing the order of rows within the table. (MySQL's InnoDB storage engine uses them for primary keys.)
- **Clustered Index:** In some database systems, such as SQL Server, a clustered index determines the physical order of the rows in the table. Each table can have only one clustered index.
- **Non-Clustered Index:** A non-clustered index is a separate structure from the table data and can be created on any column(s) in the table. Each table can have multiple non-clustered indexes.

To create an index during your `CREATE TABLE` query:

```sql
CREATE TABLE example
(
  c1 INT PRIMARY KEY,
  c2 INT NOT NULL,
  c3 INT NOT NULL,
  c4 VARCHAR(10),
  INDEX (c2,c3)
);
```

Or to add an index for a column or set of columns, you can use:

```sql
CREATE INDEX index_name
ON table_name (column_list);
```

> Make sure not to go too wild with indexes. Over-indexing can lead to increased storage overhead and slower data modification operations (such as inserts, updates, and deletes). Often maintenance on databases involves index rebuilding or reorganising in order to maximise performance.

<!-- Links -->

[sandbox]: https://coderpad.io/sandbox
[mysql-primary]: https://dev.mysql.com/doc/refman/8.0/en/create-table.html
