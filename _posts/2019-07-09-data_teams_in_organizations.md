---
title: Data teams in organizations
author: thedarkside
date: 2019-07-09 00:00:00 +0100
categories: [General]
tags: []
---

Data Science is all about gathering findings from data. It's about uncovering hidden information that allow businesses to make better decisions. Data translates business questions into data-driven answers. Today data is everywhere. And it's incredibly valuable.

# So what exactly can data do?

Data can be used to describe the state of an organization or a process at a given time using reporting like dashboards or alerts. It can be used to determine the cause of events and behaviors - with data science techniques we can examine the correlation between events and complex causal relationships. Finally, machine learning models can help in predicting the outcomes of future events.

# The Data Science workflow

Every data science project starts with data collection. Data collection methods can be divided into primary and secondary. Secondary data are all the data that have already been gathered and can be obtained from existing sources. Primary data are the data that are yet to be collected either through qualitative or quantitative collection methods. Qualitative methods include all kinds of non-quantifiable data such as feelings, words, and emotions and are gathered through interviews, surveys, focus groups, and so on. Quantitative data can be represented mathematically and various numeric methods can be applied to it.  

The next step in the workflow is data visualization and exploration. It involves building dashboards for visual inspection of data over time or for comparison of different datasets.

Finally, appropriately processed data is used for making future predictions. This stage is when actual machine learning models are created.  

At each stage of the data science workflow, various OKRs (Objective/Key Results) may appear. To present an example:

Data collection objective: Improve Data Warehousing

* KR 1: Limit the time needed to import data from an external source to 12h

* KR 2: Decrease the number of reported data import errors by implementing automatic tests

Visualization & Exploration objective: Make Company Results Reporting Transparent

* KR 1: Build a Tableau dashboard showing monthly revenues and losses

* KR 2: Prepare an automatic newsletter tracking monthly revenues and losses

Prediction model objective: Increase Revenue from Key Client

* KR 1: Implement a linear regression model to estimate the value of Key Client purchases based on historical data    


# Applications of Data Science

There are three main areas where data science is used. **Traditional machine learning models** can be used to detect fraudulent transactions on credit cards, predict customer churn, categorize potential customers into groups to adjust purchase recommendations, and so on.

Another category is the so-called **Internet of Things (IoT)**, which in general means gadgets not being standard computers but still having the ability to transmit data - e.g. smart watches monitoring physical activity, home automation devices (like lighting, air conditioning, security systems) or voice control home assistance.

Lastly, **deep learning** is used to draw conclusions from huge amounts of data (which couldn't be processed by traditional machine learning algorithms) thanks to the implementation of multiple layers of mini-algorithms ("neurons"). Such models are used for example in image recognition for autonomous driving systems.

# Data Science Team Structure

#### Data Engineer
##### Toolkit: SQL / NoSQL + Python / Java / Scala

Data engineers are mainly responsible for building architectures for data storage and processing. They form the first step in the data science workflow by extracting data from available sources and piping it somewhere for the analysts to use. Common data sources are websites, customer data, logistics data, and financial transactions. Often engineers also do transformations so that the data is _clean_ and easy to analyze. They might be responsible for a so-called data pseudonymization - a process in which personally identifiable information (PII, one of the especially sensitive kinds of data) is replaced by artificial identifiers, and access to information is restricted only to necessary groups.

#### External Data Sources
Other popular data sources are APIs, public records, and mechanical turk. Using an API (Application Programming Interface) is an easy way to request data from a third party. Some well-known APIs include _Twitter, Wikipedia, Yahoo! Finance_ and _Google Maps_. Public data are generally gathered and shared by public institutions and consist of information related to health, economics, demographic statistics, etc. Mechanical Turk (MTurk) is the outsourcing of simple data validation tasks to a big group of people (or a crowd, which results in the term _crowdsourcing_). The aim is to obtain a big enough labeled or otherwise validated dataset to be used as input in machine learning algorithms. Many platforms are offering to recruit workers for these tasks, such as _AWS MTurk_.

#### Data Storage Systems

In general, businesses generate more data that can be stored on a single computer. In such cases parallel storage solutions are used - data is stored across many different computers. A company might have its own set of storage computers called a cluster or a server on its premises or it can use the storage services of another company, which is referred to as cloud storage. Common cloud services providers include _Microsoft Azure, Amazon Web Services (AWS)_ and _Google Cloud_.

Different types of data require different storage solutions and have their query languages. Tabular data (a type of data that can be expressed as a set of rows and columns) can be stored in a relational database and SQL (Structured Query Language) is used to gain access to them. Unstructured data, like text, images, video, and audio files, are stored in a document database. Querying such databases is done mainly through NoSQL (Not Only SQL).

#### Data Analyst
##### Toolkit: SQL / NoSQL + Spreadsheets + BI Tools (Tableau, Power BI, Looker)

Data analysts describe the data through statistical analysis, hypothesis tests, and visualization. They use existing data storage platforms, developed by data engineers, to analyze and summarize data. Oftentimes they are responsible for creating dashboards using Business Intelligence (BI) tools.

#### Dashboards

Dashboards are a visual set of metrics updating either in real-time or on a fixed schedule. These may include time series plots, bar charts for categorical comparison, single number highlights (such as the number of visitors on a website), and many others. Usually, tables and big chunks of text are avoided. Among tools used for building dashboards, we will find spreadsheets like Excel or Google Sheets, BI Tools such as Power BI, Tableau, and Looker, or more sophisticated, customized tools like R Shiny and d3.js. The same dashboard should be implemented across the whole organization (or at least for a specific project) to improve consistency and readability.

#### Ad-Hoc Requests

Analysts might sometimes be asked an ad-hoc request – a request which is not repeated periodically but is rather an unpredictable initiative from other departments. Such a request should be specific and provide the context so that an analyst can spot and recommend which data to use. It additionally should have a priority and a due date. A good practice for managing ad-hoc requests is to implement a ticketing system like Trello, JIRA, or Asana where customers will be able to raise a request with all mentioned details. Then the requests are internally assigned to the members of the data analytics team.

#### A/B Testing

A/B testing is a type of randomized experiment for determining which of two options (A or B) is more popular with clients. It usually concerns changes on a website, the addition of new features in an app, various e-mail marketing campaigns, etc.  Running an A/B test consists of four steps:

- pick a metric to track, e.g. number of clicks on a website,
- calculate sample size – a larger sample size will allow the detection of smaller differences between options,
- execute the experiment,
- check for statistical significance – this will tell if the difference between two options is meaningful (i.e. it represents an actual difference in tastes and is not just caused by random chance).

#### Machine Learning Scientist
##### Toolkit: Python / R

Machine learning analysis is used to extrapolate what is likely to happen based on what is already known from historical data. ML scientists use training data for predicting outcomes of events, classification, image processing, automated text analysis, and so on.

#### Supervised vs Unsupervised Machine Learning

Supervised learning is a subset of ML models where data have features and labels. Examples of the use of such models are recommendation systems or churn prediction. Features (or variables, attributes) are various characteristics of each observation in a dataset (e.g. age, gender, income). Labels are quantities that the model will predict (e.g. whether or not the customer will churn) based on features.   

Another group of ML algorithms is unsupervised learning methods. For example, clustering divides data into groups by finding some patterns within a dataset. Examples of use include customer segmentation, image segmentation, or anomaly detection. Contrary to supervised learning models, clustering uses features but doesn't use labels.

Model evaluation is an important part of building an ML solution. Only a part of a dataset is used for training the model and the remaining part is used for testing its goodness.

#### Natural Language Processing

Natural Language Processing (NLP) in general is any ML problem where the dataset is in form of a text (e.g. reviews, e-mail subjects, tweets). Possible uses include classifying sentiment (positive or negative emotion expressed in a text) of reviews or clustering medical records with similar pathologies. To make a successful NLP model, the text has usually to be divided into separate words and then their sentiment is checked. To account for synonyms word embedding is used – a method that groups together similar words.

#### Deep Learning

Deep Learning (or Neural Networks, Neural Nets) is a special area of ML models requiring much more data but able to solve way more complex problems. Although predictions might be very accurate, it is often difficult for humans to understand why the model predicted one outcome and not another. An area of models where the prediction is understandable is called an explainable AI (where AI stands for Artificial Intelligence). Such models not only give predictions but also an explanation of why a particular prediction was made.