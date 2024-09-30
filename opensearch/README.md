## OpenSearch For The Productivity Win; Fostering Adoption Across Teams

---

## Who We Are

----

## California Digital Library (CDL)

CDL provides transformative digital library services, grounded in campus partnerships and extended through external collaborations, that amplify the impact of the libraries, scholarship, and resources of the University of California. 


----

CDL was explicitly created as a central unit within the 
UC system with a mission to coordinate, leverage, and amplify the experience, expertise, and capabilities of its partners.

----

### CDL has 4 active programs:

* Publishing & Special Collections
* Discovery & Delivery
* Collections & Licensing
* UC Curation Center (UC3)

----

## UC3

----

## UC3 Architecture

---

## Background

---

## The Log4J incident

Raised the priority to improve our logging systems to 
- be able to patch and address security vulnerabilities
- inconsistent logging across our microservices
- distributed environment

----

## Logging Needs
- Find specific error events in logs 
- Compute key event metrics
- Discover unreported errors

----

## Discussion: Your Logging Needs

---

## Why Opensearch?

----

## Find specific error events in logs 

- High avalability services (multiple instances)
- Eventual migration to containers - log directories will be less accessible

----

## Screenshot - search for error message

----

## Compute key event metrics

- How widely are functions utilized
- Frequency that functions are utilized
- Metric totals

----

## Screenshot: Count Events, Count Totals

----

## Discover unreported errors

- Aggregate counts by return code

----

## Screenshot: Filter by Return Codes

----

## Need for Consolidated Logging

- CDL had Splunk
- Developement teams did not have access
- CDL is an AWS shop

----

## Discussion: Your Logging Tools

----

## As we ingested logs...

- We realized that OpenSearch is a great visualization tool
- for OUR DATA, not just our logs

----

## As we explored data in OpenSearch

- We realized that it could be a great SEARCH tool
- for OUR DATA, not just our logs

---

## What is OpenSearch?

----

## An OpenSource fork of Elastic

- AWS is steering the development

----

## An OpenSource for of the ELK Stack

- Elastic
- Logstash
- Kibana

----

## OpenSearch

- OpenSearch
- Logstash
- OpenSearch Dashboards

----

## Other Concepts

- Elastic Common Schema (ECS)
- json standard way of generating logs
- used in open telemetry initiatives
- common language support

---

## OpenSearch - Our Learning Process

----

## Docker distribution of OpenSearch

----

## Creating a tutorial to understand the components

- https://github.com/CDLUC3/opensearch-tutorial
  
----

## OpenSearch as an AWS managed service (for logs)

- internal users only
- log data may be unsanitized - must keep secure

----

## Types of logs

- Application logs
- Load balancer logs
- Serverless application logs
- Application firewall logs
- System logs
- Logstash logs

---- 

## Discussion: what types of logs do you have

----

## Log Ingest Challengs

- Crazy regular expressions
- Application inconsistency
  - need for ECS
- Logs that are not log files

----

## Visualizing Logs

- Demo: Merritt Visualization

----

## Log Dashboards

- Visualization
- Filters
- Sample log records

----

## Demo: Merritt Application Logs

- filters
- timelines
- selected columns
- aggregate countts

---- 

## Discussion - How do you explore log files

----

## Demo: Load Balancer Logs

----

## Demo: Web Application Firewall Logs

----

## What about Data?

- Demo: Merritt cumulative ingests

----

## OpenSearch as an AWS manage service (for data)

- deploy to end users
- different security challenges

---

## UC3 Team Collaboration and Process

----

## Screenshot Merritt System Cumulative Totals

----

## Merritt System - Object Health

- [Video](https://youtu.be/QtLZpID_THo?si=kwwoDTJq_AsRauBU)

----

## EZID Demo

- add video of search tool

----

## DMP Tool - Open Search

- add video if applicable

----

## Discussion: What stories could you tell by visualizing your application data?

---

## Open Search Community Engagement

- Slack channel
- Community Meetings

---

## Helpful Resources for OpenSearch

- summarize tutorial resources
- Ashley's admin resources (index configuration)
- Ashley's admin resources (hosting)

---

## Thank You
