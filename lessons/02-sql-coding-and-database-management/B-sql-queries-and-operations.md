---
title: "SQL Queries & Operations"
description: "This section is dedicated to mastering the practical aspects of SQL, covering the execution of Data Definition Language (DDL) commands for defining database structure, Data Manipulation Language (DML) statements for retrieving, inserting, updating, and deleting data, as well as other essential SQL operations. Participants will learn how to write efficient queries, manage database objects, and perform various data manipulation tasks to extract valuable insights from databases. Through hands-on exercises and real-world scenarios, participants will develop proficiency in SQL coding and database management techniques."
---

Earlier in this lesson we learned about constraints. There is a useful mechanism that sits alongside these constraints called `AUTO_INCREMENT`. This is a MySQL-specific mechanism that generates numbers sequentially automatically. If assigned 0, NULL or just left blank, MySQL will automatically increment from the current highest value in that column.

> Please note that if you try and use auto increment for a different column you will get Error Code: 1075. Incorrect table definition; there can be only one auto column and it must be defined as a key.
