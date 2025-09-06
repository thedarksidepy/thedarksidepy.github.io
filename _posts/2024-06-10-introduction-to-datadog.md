---
title: Introduction to Datadog
author: thedarkside
date: 2024-06-10 00:00:00 +0100
categories: [Infrastructure]
tags: [Datadog, Infrastructure, Monitoring]
---

# Introduction to Datadog

### What is Datadog?

Datadog is an Application Performance Monitoring (APM) system. It monitors key aspects of an application such as availability, reliability, and scalability.

### 4 SRE (Site Reliability Engineering) golden signals

- Latency – the time it takes to service a request.
- Traffic – a measure of how much demand is being placed on your system, e.g. number of requests per second.
- Saturation - the percentage of resources consumed.
- Errors - the rate of requests that fail.

### Sending data to Datadog

Datadog provides hundreds built-in integrations.

Data (metrics, events, logs, traces, etc.) can be sent to Datadog using:
- Datadog agent
- Datadog API
- Integrations

### Datadog basic terms

**Host** - any physical or virtual operating system instance monitored with Datadog. 

**Datadog Agent** - software running on your hosts that collects events and metrics and sends them to Datadog. It acts as a middle layer between an application and the Datadog platform. 

**Tags** - metadata that adds dimensions to Datadog telemetries for filtering, aggregation, and comparison in visualizations.

**Retention period** - the number of days for which the logs remain searchable. 

### Datadog Agent

You can install the Datadog Agent by going to `Integrations > Agent`. 

After Datadog Agent is installed on your host, it immediately and automatically begins collecting events and metrics and sending them to Datadog. Data can then be searched, filtered, aggregated, and used for alerts.

There are two main components of the Datadog Agent: 
- **Collector** gathers data from host every 15 seconds.
- **Forwarder** sends collected data to Datadog over HTTPS.

Datadog Agent performs the following actions:
- Collects data at fixed time intervals
- Aggregates data across calls within the interval
- Sends aggregates to Datadog (each aggregate is a single point on a graph)

Once the Datadog Agent is installed, the main config file is created as `datadog.yaml`. Integrations config files are located in the `conf.d` folder. If you change the config files, you then need to restart the Datadog Agent for the changes to take effect. You can restart the Datadog Agent using the Datadog Manager on your host. 

### Host Map

Access the Host Map via `Infrastructure > Hosts > Host Map`. 

The Host Map lists all hosts. Colors of the tiles represent CPU utilization. Clicking on a tile reveals details such as OS, network, memory, and filesystem.

The Apps section shows blue tiles: agent, ntp, and system. New tiles appear as more integrations are enabled. Click on a tile to display related metrics.

### Integrations

Install new integrations by going to `Integrations > Integrations`. 

Integrations can be filtered by status (broken, installed and available). Each integration page includes info, setup instructions and available metrics.

### Metrics

Each metric submitted to Datadog must have a type. There are 4 different metric types in Datadog based on how data collected during a time interval gets aggregated:

- Distribution - global statistical distribution of a set of values calculated across the entire distributed infrastructure in one interval.
- Count - total number of event occurrences in one interval. 
- Rate - occurrences per second in one interval. 
- Gauge - a snapshot of events at a point in time. Can be used to take a measure of something reporting continuously, like the available disk space or memory used.

Metric types affect how the metric values are displayed when queried and determine the types of possible graphs.

Metrics can be submitted via: 
- Agent
- DogStatsId (bundled with the Agent)
- Datadog HTTP API

### Dashboards

Dashboards are used for real-time visual tracking, analyzing and displaying key performance metrics to help monitor applications health. All dashboard data can be filtered by time. 

There are 2 types of dashboards in Datadog:
- Timeboard
- Screenboard 

#### System Dashboards

Six system dashboards are created after Agent installation. Additional dashboards appear when new integrations are enabled.

### Monitors (Alerts) and Notifications

To create a new monitor navigate to `Monitors > New Monitor`. 

Datadog offers multiple monitor types. Hovering over each option shows its description.
