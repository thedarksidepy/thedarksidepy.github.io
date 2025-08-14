---
title: Apache Airflow 3 Fundamentals – Certification Exam
author: thedarkside
date: 2025-07-04 00:00:00 +0100
categories: [Apache Airflow]
tags: [Airflow]
---

## Prerequisite: Install and run Airflow using Astro

Astro CLI is an open-source, fully managed Airflow solution. It is the easiest and fastest way to set up and run Airflow locally. 

> Ask Astro: <https://ask.astronomer.io/> is the Apache Airflow's AI language model. 
{: .prompt-tip }

### Prerequisites 

Install Homebrew: <https://brew.sh/>

Install Docker: <https://www.docker.com/>

### Install the Astro CLI

Install the Astro CLI: `brew install astro`

Check that the installation was successful: `astro version`

Upgrade the Astro version: `brew upgrade astro`

Uninstall Astro: `brew uninstall astro`

### Create an Astro project

> See the full instruction: <https://www.astronomer.io/docs/astro/cli/get-started-cli>
{: .prompt-tip }

Generate new project: `astro dev init`

All required project files will automatically be created. Most important are:

- `dags` → the main folder for DAG pipelines (python scripts)
- `include` → put here any additional scripts like SQL queries or bash scripts 
- `plugins` → additional custom components like operators, hooks, sensors, etc.
- `.env` → environment variables
- `requirements.txt` → additional python packages

By default, Airflow timezone is set to UTC. This can be changed using the `AIRFLOW__CORE__DEFAULT_TIMEZONE` variable.

### Run Airflow locally

Run the project locally: `astro dev start`

This command spins up 5 Docker containers on your machine: postgres, api server, dag processor, scheduler and triggerer.

![](/assets/img/airflow-3-exam/airflow-docker-containers.png)

After your project builds successfully, open the Airflow UI in your web browser at `https://localhost:8080/`.

Restart the project: `astro dev restart`

Stop the project: `astro dev stop`

Delete the project with all metadata: `astro dev kill`

### Run Astro project from Visual Studio Code

- install the `Dev Containers` extension
- Terminal > New Terminal
- `astro dev start`
- click on the icon on the bottom left (Open a Remote Window)
- choose: Attach to Running Container
- select the scheduler container (a new window will open)
- File > Open Folder
- `/usr/local/airflow` > OK

Now, the Visual Studio Code is running inside the Docker container corresponding to the Airflow Scheduler.

To close the connection:

- click on the bar on the bottom left (the blue one with the name of the container)
- Close Remote Connection

## Topic 1: Airflow Use Cases 

### Apache Airflow is a data orchestrator

- It manages the process of moving data between various data tools.
- It coordinates and automates data flows across various tools and systems.
- It allows to programmatically author, schedule and monitor workflows.

### Brief orchestration / workflow management history
#### Pre-unix era
In the pre-Unix era, data processing was handled **manually through batch** jobs with simple scheduling. These processes were **prone to errors** and required constant operator intervention.

#### Early computing
With the arrival of early computing, **basic time-based scheduling** tools emerged — for example, **CRON** in Unix/Linux systems. Around the same time, commercial ETL tools like Informatica appeared, enabling automation of data workflows but at a high cost and with significant resource requirements.

#### Data and open-source
The next stage saw the growth of open-source data processing tools. Solutions like Luigi and Oozie became popular, enabling more complex data pipelines. However, many were tied to specific ecosystems (such as Hadoop), suffered from limited scalability, and required workflows to be defined in XML or configuration files, reducing flexibility.

#### Modern data orchestration
In the era of modern data orchestration, a major shift occurred in 2015 with the release of **Apache Airflow** — an **open-source platform** for defining **data pipelines as Python code**. Airflow supports both time-based and event-based (data-aware) scheduling and **integrates with hundreds of external systems**, making it one of the most widely adopted orchestration tools in the industry today.

> Airflow is suitable only for batch processing (not streaming) but it can be used with Kafka.  
{: .prompt-warning }

> Airflow can process (transform) data but it is not specifically designed for that (as a best practice use dedicated external systems like Spark and always carefully consider available Airflow resources). 
{: .prompt-warning }

## Topic 2: Airflow Concepts
A **DAG (Directed Acyclic Graph)** represents a **single data pipeline** — a collection of tasks arranged to reflect their execution order. Directed means that when a DAG contains multiple tasks, each task must have at least one defined upstream or downstream dependency. Acyclic means the graph cannot contain loops — execution must always move forward.

A **Task** is the smallest unit of work within a DAG and is represented as a single node in the graph.

An **Operator** defines what work a task performs. Airflow offers more than 900 built-in operators, covering a wide range of actions — from running SQL queries to transferring files between systems.

### Core Airflow components
Airflow is built around seven main components, each responsible for a specific part of the orchestration process.

![](/assets/img/airflow-3-exam/airflow-3-components.jpg)

#### Metadata Database
Stores all metadata related to the Airflow instance, including DAG definitions, task states, and execution history.

#### DAG File Processor 
A dedicated process that scans the `dags` folder for new or updated DAG files (by default, every 5 minutes). It parses these files and writes the serialized DAG definitions into the Metadata Database.

####  Scheduler 
Continuously monitors the Metadata Database (default: every 5 seconds) to determine if any tasks are ready to run.

- Creates and schedules Task Instance objects.
- Runs persistently in the background.
- Can operate on a single machine or in a distributed setup.
- Multiple schedulers can run simultaneously.
- Once started, keeps running until explicitly stopped by a user or the system.

#### API Server 
- Receives task status updates from workers and writes them to the Metadata Database.
- Serves the Airflow UI (React-based).

In Airflow 3, the Flask-based webserver and API from Airflow 2 were merged into this single API Server.

#### Executor 
Defines how and where (on which system) tasks are executed. Pushes Task Instance objects into the Queue for processing.

#### Queue
Holds tasks that are ready to run and determines their execution order.

#### Worker(s) 
- Picks up tasks from the Queue and runs the code.
- Sends task status updates back to the API Server.
- Can scale horizontally by running multiple workers workers on the same or separate clusters.

### Stages of running a DAG

1) **DAG discovery**: the DAG File Processor scans the `dags` directory for new or updated DAG files (default: every 5 minutes).
   
2) **Parsing & serialization**: when a new DAG is detected, it is parsed and serialized into the Metadata Database.

3) **Scheduling**: the Scheduler checks the Metadata Database (default: every 5 seconds) to find DAGs that are ready to run.

4) **Queuing**: once a DAG is ready to run, its tasks are placed into the Executor's queue.

5) **Task Assignment**: once a worker becomes available, it retrieves a task from the queue.

6) **Execution**: the worker runs the task code.


### Airflow providers
Core capabilities of Airflow can be extended by installing additional packages called providers. Providers are listed on <https://registry.astronomer.io/>

### Defining a DAG in Airflow

A DAG has 4 core parts:

- the import statements where the needed operators or classes are imported
- the DAG definition, where the DAG object is called and its properties are defined 
- the DAG body where tasks are defined with the operators they will run
- the dependencies where the order of execution of the tasks is defined 

There are 3 ways to declare a DAG:

1) using the `@dag` decorator

2) using the context manager (`with` statement)

3) using the standard constructor (old way, not recommended)

#### Using the DAG decorator 

```python
# Import the dag and task decorators
from airflow.sdk import dag, task

# Define any functions you need for your tasks
def _task_a():
    print("Hello from task A")

def _task_b():
    print("Hello from task B")

# Define the DAG object
@dag(schedule=None)
def my_dag_decorator():  # this will be the name (unique identifier) of the DAG 
    # Define tasks (operators)
    task_a = PythonOperator(task_id='a', python_callable=_task_a)
    task_b = PythonOperator(task_id='b', python_callable=_task_b)

# Define tasks dependencies
task_a >> task_b  # task_a is called upstream, task_b is called downstream

# Always call your dag function at the end of the file
my_dag_decorator()
```

As a best practice, keep the name od the DAG the same as the filename. 

The `@dag` decorator is a DAG factory – it returns a DAG object when called.


#### Using the context manager 

```python
# Import the DAG object
from airflow.sdk import DAG

# Define any functions you need for your tasks
def _task_a():
    print("Hello from task A")

def _task_b():
    print("Hello from task B")

# Open a DAG context
with DAG(
    dag_id="my_dag_context",
    schedule=None
):
    # Define tasks (operators)
    task_a = PythonOperator(task_id='a', python_callable=_task_a)
    task_b = PythonOperator(task_id='b', python_callable=_task_b)

# Define tasks dependencies
task_a >> task_b
```


#### Using the standard constructor (old way, not recommended)

```python
# Import the DAG object
from airflow.sdk import DAG

# Define any functions you need for your tasks
def _task_a():
    print("Hello from task A")

def _task_b():
    print("Hello from task B")

# Define the DAG object
my_dag_standard = DAG(
    dag_id="my_dag_standard",
    schedule=None
)

# Define tasks (operators)
task_a = PythonOperator(task_id='a', python_callable=_task_a, dag=my_dag_standard)
task_b = PythonOperator(task_id='b', python_callable=_task_b, dag=my_dag_standard)

# Define tasks dependencies
task_a >> task_b
```

Note how you need to assign every task to your DAG.

> Be careful to give unique names to all your DAGs. If two DAGs share the same name, Airflow will randomly parse one of them. 
{: .prompt-danger }


### Mandatory and optional DAG parameters 

#### dag_id

The only mandatory DAG parameter. 

- Must be set explicitly when using the `DAG` class.
- If using the `@dag` decorator, the function name is used as the `dag_id` by default.

While `dag_id` is the only required parameter, there are several recommended optional parameters. Their default values and behavior can vary depending on your Airflow version and configuration.

#### start_date
Specifies when the DAG should begin scheduling. Also determines the earliest timestamp from which the scheduler will attempt backfilling.

```python 
from pendulum import datetime

@dag(
    start_date=datetime(2024, 1, 1),
    ...)
```

#### schedule
Defines how often the DAG runs. Common values include:

- `None` → no schedule; the DAG must be triggered manually (via UI, API or CLI)

- `@daily` → run once per day at midnight

- `@continuous` → run immediately after the previous run finishes (note: this is **not** real-time processing)

📄 Reference: <https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/cron.html#cron-presets>

You can also use a `duration` object to schedule runs at fixed intervals. Example — run every 3 days:

```python 
from pendulum import duration 

@dag(
    schedule=duration(days=3),
    ...)
```

If `schedule` is set, the `start_date` becomes mandatory.

#### catchup
Controls whether the scheduler should run all missed DAG runs from the past (since the `start_date`).

- `catchup=True` → scheduler backfills all non-triggered runs

- From Airflow 3, the default is `CATCHUP_BY_DEFAULT = False`

- Can be set globally or per DAG

### default_args
Arguments applied to all tasks within a DAG (can be overridden at the task level). Defined as a dictionary:

```python
default_args={
    "depends_on_past": False,
    "retries": 1,
    (...)
```

### DAG Runs
A DAG Run is a single execution instance of a DAG. Every time the DAG is triggered — whether manually or via its schedule — a new DAG Run object is created.

Key DAG Run properties:

- `run_id` → a unique identifier of a DAG Run
- `data_interval_start` → the start of the data interval the DAG Run covers
- `logical_date` → the logical execution date of the DAG Run (often the same as `data_interval_start`)
- `data_interval_end` → the end of the data interval the DAG Run covers

Possible DAG Run states:

- `queued` → waiting to start
- `running` → currently executing tasks
- `success` / `failed` → completed with or without errors

> By default, up to 16 DAG Runs of the same DAG can run simultaneously. 
{: .prompt-info }

Active DAGs → unpaused DAGs that are ready to be scheduled.

Running DAGs → DAGs that currently have at least one active DAG Run in progress.

## Topic 3: Dependencies

### Defining tasks

In Airflow 3, there is a new way of defining tasks using `@task` decorator. 


