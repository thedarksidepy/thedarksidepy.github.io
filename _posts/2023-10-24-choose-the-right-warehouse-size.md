---
title: Choose The Right Warehouse Size
author: thedarkside
date: 2023-10-24 00:00:00 +0100
categories: [Snowflake]
tags: [Snowflake]
---

Snowflake warehouse size impacts **costs** and **query performance**. 

Snowflake warehouse is a **cluster of computational resources** used for running queries and tasks. It functions as an on-demand resource, separate from any data storage system, and is similar to a virtual machine (VM).

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

Until you find the sweet spot of maximum performance at the lowest cost. 

If the cost of running the warehouse doubles with each increase in size, then you would ideally want the query execution time to be at least halved to justify the increased cost from a performance standpoint. 🤔
    
$$\Rightarrow$$ Choose the size that offers the best cost-to-performance ratio. 🤔

### Automate Warehouse Suspension + Resumption

To strike a balance between performance and cost, it is suggested to use a 60-second auto-suspend instead of the default 600 seconds? ❌

That depends because...

{: .important } 
Stopping and restarting warehouses within the first minutes leads to multiple charges because the one-minute minimum charge applies each time you restart.

After a minute’s usage, all subsequent usage resumes on a per-second billing as long as your run the virtual warehouses continuously.

And also...

Note that when a warehouse suspends, its local cache gets cleared. Therefore, if there are repeating queries that scan the same tables, setting the warehouse auto-suspend too small will lead to a drop in performance.

### Check if the warehouse is under or over provisioned

An under-provisioned Snowflake warehouse may not have sufficient resources to handle the workload, leading to sluggish query performance and potential bottlenecks. To identify under-provisioning, monitor performance indicators such as **query execution time**, **queue time**, and **the number of queued queries**. If these metrics consistently show poor performance, increasing the warehouse size to allocate more resources may be necessary.

An over-provisioned Snowflake warehouse may have more resources than required, resulting in unnecessary costs without providing any significant performance improvements. To identify over-provisioning, analyze the warehouse's resource utilization, such as **CPU** and **memory usage**. If these metrics consistently show low utilization, it may be more cost-effective to reduce the warehouse size.

### Monitor disk spillage
It's crucial to monitor both local and remote disk spillage. In Snowflake, **when a warehouse cannot fit an operation in memory**, it starts spilling data first to the local disk of a warehouse node, and then to remote storage. This process, called disk spilling, leads to decreased performance and can be seen in the query profile as "Bytes spilled to local/remote storage." When the amount of spilled data is significant, it can cause noticeable degradation in warehouse performance.

To decrease the impact of spilling, the following steps can be taken:

- Increase the size of the warehouse, which provides more memory and local disk space.
- Review the query for optimization, especially if it's new query.
- Reduce the amount of data processed, such as improving partition pruning or projecting only the needed columns.
- Decrease the number of parallel queries running in the warehouse.

### Determine optimal costs and performance (find the sweet spot)

To achieve the optimal balance between performance and cost, start with an X-SMALL warehouse and gradually scale it up until the query duration stops halving. This indicates that the warehouse resources are fully utilized and helps you identify the sweet spot of maximum performance at the lowest cost.

### Review Snowflake query history for errors

Look out for error messages such as **"Warehouse full"** or **"Insufficient credit"**, which can indicate that the warehouse is unable to accommodate the query workload.
