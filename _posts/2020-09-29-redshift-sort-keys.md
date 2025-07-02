---
title: When to use Redshift sort keys?
author: thedarkside
date: 2020-09-29 00:00:00 +0100
categories: [AWS]
tags: [AWS, Redshift]
---

Sort keys determine the order in which rows in a table are stored. In general, Amazon Redshift stores columnar data in 1 MB disk blocks. It also stores metadata about every one of these blocks, including the minimum and maximum values within the block. When you run a query that restricts result data to some range, the query processor can use these minimum and maximum values to rapidly skip over a large number of blocks during the table scan. For example, if you have a table with 5 years of data sorted by date and you want to query only for one month, around 98% of the disk blocks can be eliminated from the scan. If this data was not sorted, way more blocks (or possibly all of them) would have to be scanned.

Sort keys have to be determined during the creation of a table (information about them is included in the DDL). Then, when data is initially loaded into the table, the rows are stored on the disk in sorted order. Information about sort keys is passed to the query planner and so the query performance is greatly improved.

## Compound Sort Keys

There are two types of sort keys in Amazon Redshift. The first one is a compound sort key. It is made up of all the columns listed in the sort key definition in the order they are listed. This ordering is very important, as using such a kind of key makes sense only if you filter or join data using these columns in this order. If you want to run a query using a secondary column without referencing the primary one, the benefits of using such a key decrease. 

Using compound sort keys speeds up joins, `GROUP BY`, and `ORDER BY` operations, as well as window functions that use `PARTITION BY` and `ORDER BY`. They can also help improve compression.

As per AWS documentation, the rule of thumb is:
- if recent data is queried most frequently, specify the timestamp column as the leading column for the sort key
- if you do frequent range filtering or equality filtering on one column, specify that column as the sort key
- if you frequently join a (dimension) table, specify the join columns as the sort key


## Interleaved Sort Keys

The interleaved sort key assigns equal weight to each column (or a subset of columns) in the sort key. This is the main difference from the compound sort key, where the order of columns within the key matters. Use this type of key if you have multiple queries that use different columns to filter data. 

Additional resources you might find useful:

_[AWS Documentation about vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html)_

_[Tuning table design - step by step tutorial with real-life examples](https://docs.aws.amazon.com/redshift/latest/dg/tutorial-tuning-tables.html)_

_[Detailed step-by-step analysis how to select sort keys based on your needs](https://aws.amazon.com/blogs/big-data/amazon-redshift-engineerings-advanced-table-design-playbook-compound-and-interleaved-sort-keys/)_

