---
title: Managing users and privileges in Amazon Redshift 
author: thedarkside
date: 2021-01-12 00:00:00 +0100
categories: [AWS]
tags: [AWS]
---

## Privileges 
The user that created a database object is its **owner**. Only this user has the privilege to modify or destroy the object and grant privileges on it to other users or groups of users. If you run a `CREATE TABLE` statement you will automatically become an owner of this table. To grant a privilege use the `GRANT` command, to revoke a privilege use the `REVOKE` command.

Amazon Redshift supports the following privileges:
- `SELECT` - select data from a table or a view
- `INSERT` - load data into a table
- `UPDATE` - update a table column (requires `SELECT` privilege)
- `DELETE` - delete a data row from a table (requires `SELECT` privilege)
- `REFERENCES` - create a foreign key constraint (has to be granted on both referenced and referencing table)
- `CREATE` - create database objects
- `TEMPORARY` - create temporary tables in the specified database (needed to run Amazon Redshift Spectrum queries)
- `USAGE` - granted on specific schema, makes objects in that schema accessible

## Superusers

Database superusers have the same privileges as database owners. A superuser can create another superuser. Since superusers always have all privileges, regardless of any `GRANT` or `REVOKE` commands, you should be very careful when using them. Especially when your daily work with the database is not strictly connected with database security management, you should not perform operations using the superuser role. 

To create a new superuser, you have to be logged in as a superuser and run the command `CREATE USER username CREATEUSER PASSWORD password;`. The `CREATEUSER` keyword indicates that a superuser is created (the default is `NOCREATEUSER`).

## Users

Amazon Redshift user accounts can be **created and dropped only by a database superuser**. Superusers can own databases and database objects and grant or revoke privileges on those objects to other users or groups of users. Additionally, users with `CREATE DATABASE` permissions can create databases and grant or revoke access to them.

To create a user, run the command `CREATE USER username PASSWORD password;`. 

To remove an existing user, run the command `DROP USER username` (or to remove multiple users `DROP USER username1, username2, username3`). Note that you cannot drop a user if it owns any database objects (such as a table, view, schema, or database). On the attempt of dropping such a user, Redshift will return an error. However, Redshift checks only the current database before dropping the user. Therefore, the user can own objects in another database and still be dropped. The owner of these objects will be changed to `unknown`. 

To make changes to an existing user account, use the `ALTER USER` command. Possible modifications are: changing the level of access the user has to the Amazon Redshift system tables and views, disabling the option for a user to change its password, renaming a user, setting an expiration date on a user, or limiting the number of database connections a user is permitted to have open concurrently. 

To view information about users, query the system table `pg_user`.

[PostgreSQL 12 Documentation: pg_user](https://www.postgresql.org/docs/12/view-pg-user.html)

`sql
SELECT *
FROM pg_user
ORDER BY usename
;
`

To view **all users that are currently running processes** (you must have superuser privileges to see other users' processes, otherwise you will see your own processes only) query the system view `pg_stat_activity`.

[PostgreSQL 12 Documentation: The Statistics Collector: pg_stat_activity](https://www.postgresql.org/docs/12/monitoring-stats.html#PG-STAT-ACTIVITY-VIEW)

`sql
SELECT DISTINCT usename
FROM pg_stat_activity
;
`

## Groups

Groups are collections of users. Whatever privileges are granted to a group, they are granted to all members of this group. 

To view all user groups, query the `pg_group` system catalog table.

[PostgreSQL 12 Documentation: pg_group](https://www.postgresql.org/docs/12/view-pg-group.html)

`sql
SELECT *
FROM pg_group
ORDER BY groname
;
`

Only a superuser can create, alter or drop groups. To create a group and assign users to it run `CREATE GROUP groupname WITH USER username1, username2`. 

To add a user to the group run `ALTER GROUP groupname ADD USER username`. To remove a user from the group run `ALTER GROUP groupname DROP USER username`. Finally, to rename an existing group run `ALTER GROUP groupname RENAME TO new_groupname`.

To delete a user group use the command `DROP GROUP groupname`. This command deletes the group, but not individual users. A group cannot be dropped if it has any privileges on any objects. To delete such a group, the privileges have to be revoked first. 

## Schemas

A database contains one or more schemas. Each schema has its objects like tables and views. By default, a database has a schema called `PUBLIC`. Schemas are similar to file system directories, except that schemas cannot be nested.

Examples of how the usage of schemas can help with organization and concurrency issues:
- many developers can work in the same database without interfering with each other
- database is organized into logical groups 
- names given to objects used by one application do not collide with the names of objects used by other applications

To define a new schema in a database, use the command `CREATE SCHEMA schema_name` (obviously the name cannot be `public`). To give the ownership of a schema to a specific user run `CREATE SCHEMA schema_name AUTHORIZATION username`. Amazon Redshift allows for a maximum of 9900 schemas to be created within a database.

All schemas created in a database are listed in a `pg_namespace` catalog table. 

[PostgreSQL 12 Documentation: pg_namespace](https://www.postgresql.org/docs/12/catalog-pg-namespace.html)

`sql
SELECT *
FROM pg_namespace
ORDER BY nspname
;
`

To change the definition of an existing schema run `ALTER SCHEMA schema_name`. Possible operations include renaming a schema and changing the owner.

To delete a schema run `DROP SCHEMA schema_name`. Adding a `CASCADE` keyword ensures that all objects within a schema are deleted as well. The `RESTRICT` keyword prevents a schema from being deleted if it contains any objects. 

To create a table within a schema create the table with the format `schema_name.table_name`. To list all tables that belong to a particular schema query `pg_table_def` system catalog table. 

`sql
SELECT DISTINCT
    tablename
FROM pg_table_def
WHERE schemaname = 'public'
;
`

By default, all users have `CREATE` and `USAGE` privileges on the `public` schema. The `REVOKE` command can be used to remove the privilege of creating new objects in the `public` schema.

#### Search Path

A search path is basically a comma-separated list of existing schema names. This list specifies in which schemas (and in what order) Redshift will search for an object (such as a table or a view) if you reference it without an explicit schema component. Also, when objects are created without a specific target schema they are placed in the first schema within the search path. If the search path is empty, the system will return an error. If an object exists in a schema that is not listed in the search path, it can be referenced only by specifying its schema using dot notation. The system catalog schema `pg_catalog` is always searched. If it is listed in the search path, it is searched in the order specified there. If not, it will be searched before any schemas specified in the search path. 