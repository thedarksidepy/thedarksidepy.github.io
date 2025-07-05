---
title: Apache Airflow 3 Fundamentals – Certification Exam
author: thedarkside
date: 2025-07-04 00:00:00 +0100
categories: [Airflow]
tags: [Airflow]
---

# Topic 1: Airflow Use Cases 

## Airflow is a data orchestrator. 

↳ It manages the process of moving data between various data tools. 

↳ It coordinates and automates data flows across various tools and systems. 

↳ It allows to programmatically author, schedule and monitor workflows.

## Brief orchestration / workflow management history:
1) Pre-unix era:

↳ manual batch processing and scheduling

↳ error prone

2) Early computing:

↳ basic time-based scheduling (e.g. CRON for Linux)

↳ rise of ETL tooling (e.g. Informatica) → expensive and resource intensive

3) Data and open-source:

↳ rise of tools like Luigi or Oozie

↳ limitations: some tools working only with Hadoop ecosystem, limited scalability, XML or config files used to define workflows 

4) Modern data orchestration:

↳ Apache Airflow (2015)

↳ open-source

↳ pipelines as code in Python

↳ integration with hundreds of external systems

↳ time and event-based (data aware) scheduling 

> Airflow is suitable only for batch processing (not streaming) but it can be used with Kafka.  
{: .prompt-warning }

> Airflow can process (transform) data but it is not specifically designed for that (as a best practice use dedicated external systems like Spark and always carefully consider available Airflow resources). 
{: .prompt-warning }

# Topic 2: Airflow Concepts

DAG = Directed Acyclic Graph → a single data pipeline

Task → a single unit of work in a DAG → represented by a single node

Operator → defines the work a task does → there are 900+ built-in operators

## Core Airflow components

![](/assets/img/airflow-3-exam/airflow-3-components.jpg)

### DAG File Processor 
A dedicated process for **parsing DAG files** from the `dags` folder. By default looks for new DAGs to parse **every 5 minutes**. 

Serialized DAG file is written to Metadata Database.

###  Scheduler 
**Schedules tasks** when the dependencies are met. 

↳ by default reads from the Metadata Database every 5 seconds to check if there are any tasks to run

↳ creates and schedules Task Instance objects

↳ designed to run as a persistent service (continuously run in the background for an extended period)

↳ can be run on a single machine or distributed across multiple machines

↳ multiple schedulers can run at the same time

↳ once scheduler is started, it continues to run until explicitly stopped manually or by the system

### API Server 
API Server takes information from workers about **tasks statuses** and **updates the Metadata Database**.

API Server also **serves the Airflow UI** (React-based).

In Airflow 3, the flask-based Webserver and API logic from Airflow 2, was merged into a single API Server.

### Executor 
Defines **how tasks are executed and on which system**. 

Pushes the Task Instance objects into Queue.

### Queue
Defines the **execution order**.

### Worker 
**Executes tasks**. Picks up Task Instance objects from the Queue and runs the code. 

Communicates the Tasks statuses to the API Server. 

There can be multiple workers. They can be put in a separate cluster. 

### Triggerer 
Process running asyncio to support deferrable operators. 


## Airflow providers
Core capabilities of Airflow can be extended by installing additional packages called providers. 

