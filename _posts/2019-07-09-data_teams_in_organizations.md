---

title: Data teams in organizations
author: thedarkside
date: 2019-07-09 00:00:00 +0100
categories: [other]
tags: [other]

---

Data Science is all about gathering findings from data. It's about uncovering hidden information that allows businesses to make better decisions. Data translates business questions into data-driven answers. Today data is everywhere, and it's incredibly valuable.

# So what exactly can data do?

Data can describe the state of an organization or a process at a given time using reporting like dashboards or alerts. It can determine the cause of events and behaviors. With data science techniques, we can examine the correlation between events and complex causal relationships. Finally, machine learning models can help predict the outcomes of future events.

# The Data Science workflow

Every data science project starts with data collection. Data collection methods can be divided into primary and secondary. Secondary data are data already gathered and obtainable from existing sources. Primary data are those yet to be collected, either through qualitative or quantitative methods. Qualitative methods include non-quantifiable data such as feelings, words, and emotions, gathered through interviews, surveys, or focus groups. Quantitative data can be represented mathematically and analyzed with numeric methods.

The next step is data visualization and exploration. This involves building dashboards for visual inspection of data over time or for comparison of different datasets.

Finally, appropriately processed data is used for making predictions. This is when actual machine learning models are created.

At each stage of the workflow, various OKRs (Objectives/Key Results) may appear. For example:

Data collection objective: Improve Data Warehousing

* KR 1: Limit the time needed to import data from an external source to 12h
* KR 2: Decrease the number of reported data import errors by implementing automatic tests

Visualization & Exploration objective: Make Company Results Reporting Transparent

* KR 1: Build a Tableau dashboard showing monthly revenues and losses
* KR 2: Prepare an automatic newsletter tracking monthly revenues and losses

Prediction model objective: Increase Revenue from Key Client

* KR 1: Implement a linear regression model to estimate the value of Key Client purchases based on historical data

# Applications of Data Science

There are three main areas where data science is used. **Traditional machine learning models** can detect fraudulent credit card transactions, predict customer churn, categorize customers into groups, and adjust purchase recommendations.

Another category is the **Internet of Things (IoT)**, meaning gadgets that are not standard computers but still transmit data—for example, smart watches monitoring physical activity, home automation devices (lighting, air conditioning, security systems), or voice-controlled home assistants.

Lastly, **deep learning** is used to draw conclusions from massive amounts of data that traditional machine learning cannot handle. These models use multiple layers of mini-algorithms ("neurons"). Applications include image recognition for autonomous driving systems.

# Data Science Team Structure

#### Data Engineer

##### Toolkit: SQL / NoSQL + Python / Java / Scala

Data engineers build architectures for data storage and processing. They form the first step in the workflow by extracting data from sources and piping it for analysts to use. Common sources are websites, customer data, logistics data, and financial transactions. Engineers also perform transformations so data is *clean* and easy to analyze. They might be responsible for pseudonymization, a process where personally identifiable information (PII) is replaced with artificial identifiers and access is restricted to necessary groups.

#### External Data Sources

Other sources include APIs, public records, and Mechanical Turk. An API (Application Programming Interface) is a way to request data from a third party. Examples include *Twitter, Wikipedia, Yahoo! Finance*, and *Google Maps*. Public data are gathered and shared by institutions and include information on health, economics, and demographics. Mechanical Turk (MTurk) is outsourcing simple validation tasks to large groups of people (crowdsourcing). The goal is to create a large enough labeled dataset for machine learning. Platforms offering this include *AWS MTurk*.

#### Data Storage Systems

Businesses generate more data than a single computer can store. In such cases, parallel storage solutions are used, where data is distributed across many computers. A company might have its own cluster or use cloud storage services. Providers include *Microsoft Azure, Amazon Web Services (AWS)*, and *Google Cloud*.

Different data types require different storage solutions and query languages. Tabular data (rows and columns) can be stored in relational databases accessed with SQL (Structured Query Language). Unstructured data, like text, images, video, and audio, are stored in document databases and queried mainly through NoSQL (Not Only SQL).

#### Data Analyst

##### Toolkit: SQL / NoSQL + Spreadsheets + BI Tools (Tableau, Power BI, Looker)

Data analysts describe data through statistical analysis, hypothesis testing, and visualization. They use storage platforms developed by engineers to analyze and summarize data. They are often responsible for creating dashboards using Business Intelligence (BI) tools.

#### Dashboards

Dashboards are visual sets of metrics that update in real time or on a schedule. They may include time series plots, bar charts, or single-number highlights (e.g. website visitors). Tables and large text blocks are usually avoided. Tools include Excel, Google Sheets, Power BI, Tableau, Looker, or custom tools like R Shiny and d3.js. Dashboards should be standardized across the organization or project for consistency.

#### Ad-Hoc Requests

Analysts may receive ad-hoc requests—unplanned, non-repetitive tasks from other departments. Such requests should be specific, provide context, and include a priority and due date. A good practice is to manage them through ticketing systems like Trello, JIRA, or Asana. Requests are then assigned internally to analytics team members.

#### A/B Testing

A/B testing is a randomized experiment for determining which of two options (A or B) performs better. It often concerns website changes, app features, or e-mail campaigns. Running an A/B test involves:

* picking a metric to track (e.g. clicks),
* calculating sample size (larger sizes detect smaller differences),
* executing the experiment,
* checking for statistical significance to see if the difference is real or random.

#### Machine Learning Scientist

##### Toolkit: Python / R

Machine learning analysis extrapolates what is likely to happen based on historical data. ML scientists use training data for predictions, classification, image processing, text analysis, and more.

#### Supervised vs Unsupervised Machine Learning

Supervised learning uses features and labels. Examples: recommendation systems or churn prediction. Features are characteristics of observations (e.g. age, gender, income). Labels are the quantities to predict (e.g. churn).

Unsupervised learning does not use labels. For example, clustering groups data by patterns. Applications include customer segmentation, image segmentation, and anomaly detection.

Model evaluation is key. A dataset is split into training and testing parts to check performance.

#### Natural Language Processing

Natural Language Processing (NLP) covers ML problems where the dataset is text (reviews, e-mail subjects, tweets). Applications include sentiment classification or clustering medical records. Text usually must be tokenized into words. To handle synonyms, word embeddings group similar words.

#### Deep Learning

Deep Learning (Neural Networks) is a branch of ML requiring more data but solving more complex problems. Predictions are often accurate but hard to interpret. Explainable AI addresses this by providing reasons for predictions alongside the outcomes.
