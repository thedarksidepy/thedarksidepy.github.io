---
title: CTEs and temporary tables
author: thedarkside
date: 2019-10-02 00:00:00 +0100
categories: [SQL]
tags: [SQL]
---


# Common Table Expressions

**CTEs (Common Table Expressions)** are named temporary result sets that can be referenced within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. They are created using `WITH` clause with one or more comma-separated subclauses. Each subclause provides a subquery that produces a named result set.

From Amazon Redshift documentation: `WITH` clause subqueries are an efficient way of defining tables that can be used throughout the execution of a single query. In all cases, the same results can be achieved by using subqueries in the main body of the SELECT statement, but `WITH` clause subqueries may be simpler to write and read. Where possible, `WITH` clause subqueries that are referenced multiple times are optimized as common subexpressions; that is, it may be possible to evaluate a `WITH` subquery once and reuse its results.

The basic syntax for creating a CTE is as follows:


```sql
WITH cte_name (column1, column2, ...) AS (
SELECT ...
)
```

After the keyword `WITH`, you specify the name of the CTE and the columns it will return. Then, after the keyword `AS`, you specify the query that defines the CTE.

The `WITH` clause is a tool for materializing subqueries to avoid having to recompute them multiple times. A CTE can be referenced multiple times in the same query and can also be self-referencing. Besides making the query execution faster, subquery factoring also makes complex queries easier to read (especially when we get rid of multiple chunks of identical subqueries). Defining more than one CTE within a `WITH` statement can help simplify very complicated queries which are ultimately joined together.  

The CTE is not stored as an object and the result set exists only throughout the query execution (it has an execution scope of a **single** SELECT, INSERT, UPDATE, or DELETE statement and cannot be reused after). It can be treated as a substitute for a view if you do not have permissions to create a view object or just do not want to create it.

_[Amazon Redshift Documentation](https://docs.aws.amazon.com/redshift/latest/dg/r_WITH_clause.html)_

# Temporary Tables

**Temporary Tables** exist and hold data for the duration of a session and are automatically dropped at the end of it. Typically they are created so that they can be joined into a query later in the session.    

A temporary table can have the same name as an existing permanent table (not recommended though). It is created in a separate, session-specific schema (schema name cannot be specified by a user). This schema then becomes the first schema in the search path, so the temporary table will take precedence over the permanent table unless you qualify the table name with the schema name to access the permanent table.

The syntax for creating a temporary table is the same as for permanent tables with additional `TEMPORARY` or `TEMP` keyword.

```sql
CREATE TEMPORARY | TEMP TABLE
...
```

Temporary tables are often used for **staging** raw or unprocessed data that will be used to make changes to the target table. They also allow for indexes and constraints which are not possible with CTEs.

_[Amazon Redshift Documentation](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html)_

# Which one to use?

That depends on the specific use case. 

I recommend this [StackOverflow discussion](https://dba.stackexchange.com/questions/13112/whats-the-difference-between-a-cte-and-a-temp-table/13117#13117) to get some more details.