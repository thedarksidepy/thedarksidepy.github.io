---
title: Choosing the right Snowflake warehouse size
author: thedarkside
date: 2023-10-24 00:00:00 +0100
categories: [Snowflake]
tags: [Snowflake]
---
Selecting the right Snowflake warehouse size is a balance between query performance and cost. This guide explains how warehouse sizing works, how to start small, when to scale, and what metrics help you decide.

Snowflake warehouse is a **cluster of computational resources** used for running queries and tasks. It functions as an on-demand resource, separate from any data storage system. Each size (X-Small, Small, Medium, etc.) doubles the compute power of the previous one and costs twice as much. 

{: .important } 
All warehouses, regardless of size, are **charged based on the amount of time they are running, whether actively processing queries or waiting for one to be issued**. 

_Snowflake charges **a minimum of 60 seconds per query** on a running or resized warehouse. Even if a query runs for only a few seconds, the user will be charged for a full minute of usage. Also, Snowflake offers **serverless compute** and **cloud service compute**, which have different credit structures._

The number of nodes available for a query:

| warehouse size | number of nodes | cost (credits per hour) | cost (credits per second) |
|----------------|-----------------|------|---------|
| XS             | 1               | 1    | 0.0003  |
| S              | 2               | 2    | 0.0006  | 
| M              | 4               | 4    | 0.0011  |
| L              | 8               | 8    | 0.0022  |
| XL             | 16              | 16   | 0.0044  |
| 2XL            | 32              | 32   | 0.0089  |
| 3XL            | 64              | 64   | 0.0178  |
| 4XL            | 128             | 128  | 0.0356  |
| 5XL            | 256             | 256  | 0.0711  |
| 6XL            | 512             | 512  | 0.1422  |

**Each node has 8 cores/threads**, irrespective of the cloud provider.

{: .important } 
Both the number of nodes and the cost **double** with each increase in warehouse size.

## Steps to effectively right-size the virtual warehouse:

### Start small and scale up as needed
Always begin with the smallest warehouse. Scale up if:

- Queries queue frequently.

- Execution times remain too long.

- You see memory spill (data spilled to local or remote disk).

Scaling up makes sense only if the larger warehouse is at least twice as fast, since cost doubles each step. Choose the size that offers the best cost-to-performance ratio.

### Automate Warehouse Suspension and Resumption

By default, Snowflake suspends warehouses after 600 seconds of inactivity. Lowering this to 60 seconds often reduces cost, but consider the trade-off: suspending a warehouse clears its local cache. Therefore, if there are repeating queries that scan the same tables, setting the warehouse auto-suspend too small will lead to a drop in performance.

### Signs of Under- or Over-Provisioning

An under-provisioned Snowflake warehouse may not have sufficient resources to handle the workload, leading to sluggish query performance and potential bottlenecks. To identify under-provisioning, monitor performance indicators such as **query execution time**, **queue time**, and **the number of queued queries**. If these metrics consistently show poor performance, increasing the warehouse size to allocate more resources may be necessary.

An over-provisioned Snowflake warehouse may have more resources than required, resulting in unnecessary costs without providing any significant performance improvements. To identify over-provisioning, analyze the warehouse's resource utilization, such as **CPU** and **memory usage**. If these metrics consistently show low utilization, it may be more cost-effective to reduce the warehouse size.

### Monitor disk spillage
It's crucial to monitor both local and remote disk spillage. In Snowflake, **when a warehouse cannot fit an operation in memory**, it starts spilling data first to the local disk of a warehouse node, and then to remote storage. This process, called disk spilling, leads to decreased performance and can be seen in the query profile as "Bytes spilled to local/remote storage." When the amount of spilled data is significant, it can cause noticeable degradation in warehouse performance.

To decrease the impact of spilling, the following steps can be taken:

- Increase the size of the warehouse, which provides more memory and local disk space.
- Review the query for optimization, especially if it's a new query.
- Reduce the amount of data processed, such as improving partition pruning or projecting only the needed columns.
- Decrease the number of parallel queries running in the warehouse.

### Determine optimal costs and performance (find the sweet spot)

To achieve the optimal balance between performance and cost, start with an X-Small warehouse and gradually scale up until the query duration stops halving. This indicates that the warehouse resources are fully utilized and helps you identify the sweet spot of maximum performance at the lowest cost.

### Review Snowflake query history for errors

Look out for error messages such as **"Warehouse full"** or **"Insufficient credit"**, which can indicate that the warehouse is unable to accommodate the query workload.

### Summary

1. Start with the smallest warehouse.

2. Scale up only when metrics show consistent queuing, long execution times, or memory spill.

3. A larger warehouse must deliver at least 2x speedup to justify its cost.

4. Adjust auto-suspension to minimize idle charges, but balance against cache benefits.

5. Monitor queue time, execution time, and spill metrics to guide decisions.

6. Consider micropartition usage and concurrency patterns before resizing further.

Right-sizing warehouses is an iterative process and requires monitoring and gradual adjustments.
