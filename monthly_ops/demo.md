## Adopting Monthly Operational Review Meetings as a Learning Exercise

- Terry Brady, Software Developer

https://github.com/terrywbrady

---

## Presentation Purpose

- Development team needed to take on new operational responsibilities
- Our approach to this challenge
- What we learned


---

## Merritt Team

[Merritt](merritt.cdlib.org) is a Digital Preservation Service that is run by the [California Digital Library](cdlib.org)

----

## Merritt System

- 1.4 PB of Cloud Storage (3 distinct providers)
- 4.5M Objects
- 188M Files
- 7 microservices, 26 Servers
- MySQL, ZooKeeper
- https://github.com/cdluc3/mrt-doc

----

## Team Roles

- 1 Product Manager
- 3 Software Developers
- 1 DevOps engineer (supporting multiple teams within our department)

----

## Our Backgrounds

- Primarily software development (Java, Ruby, SQL)
- One developer has a Systems Admin background
- Our DevOps engineer has a Systems Admin background
- CDL has 3 full-time Systems Administrators supporting multiple departments

----

## DevOps Adoption at CDL

- We are adopting a DevOps approach
- Need to empower developers to perform operational functions
  - without turning them into full time Systems Administrators

----

## Immediate Needs for the Development Team to Take On

- Capacity Planning
- Software End of Life
- Software Vulnerabilities/Dependencies
- Proactive Error Discovery

----

## Not our Expertise

- Need to learn functions that have not traditionally been part of software development
- While still primarily focusing on software development

----

## Discussion

- Does this challenge sound familiar?
- How has your team addressed this challenge?

---

## Our Approach

----

## Let's Figure it Out Together!

- Hold each other accountable
- Learn together
- Hopefully make it more interesting
- Iteratively improve our approach

----

## 3 Areas of Focus

- Capacity Planning - Server Performance
- Software Dependency Review
  - Vulnerabilities
  - End of Life
- Proactive Error Log Tracking

----

## Our Process

- Schedule time once a month for each area of focus
  - Limit to 30 minutes
  - Accomplish as much as we can
- Figure it out together
- Document learnings
  - Build a script to follow in next session


---

## Capacity Planning - Server Performance

----

## Our Meeting Script

- The actual script resides in a private repository.  
- A [sanitized version](https://github.com/CDLUC3/uc3-present/blob/main/monthly_ops/routine_librato_checks.md) is provided here.


----

## Review Monthly Stats

- Identify peak processing

----

Bytes Processed over 30 days

<img alt="Bytes ingested over a 30 day period with a significant peak on Sept 05" src="images/bytes.png">

----

## Sample Performance Charts

----

## Database

<img alt="Graph of database CPU over the same 30 day period.  The graph shows very modest CPU usage" src="images/database.png">

----

## Query Performance

- Our system administrators have enabled Query Performance Insights for the most recent 7 days 
- Our query performance is quite stable at this time

----

RDS Performance Insights

<img alt="Graph of RDS Performance Insights over the past 7 days.  Graph shows consistent performance." src="images/rds_performance_insights.png">


----

## Store/Ingest - subject to peak processing

- Ingest service: download and validate
- Storage service: upload to S3 and validate

----

## Server Notes

- Link to performance charts
- Expected performance
- Key items to review

----

## Storage Service

- Look for sustained CPU near 100%
- Look for peaks in SAR NFS wait
- Look for any significant drops in available memory or high swap

----

Storage Server - CPU

<img  alt="Graph of CPU usage by Storage service over the same 30 day (Aug 11 to Sep 10) period with a peak in early Sept" src="images/store_cpu.png">

----

Storage Server - IO Wait

<img alt="Graph of IO wait on the Storage service over the same 30 day period with a peak in early Sept" src="images/store_sar-io.png">

----

Storage Server - NFS wait

<img alt="Graph of NFS wait on the Storage service over the same 30 day period with a peak in early Sept" src="images/store_sar-nfs.png">

----

Storage Server - Memory Headroom

<img alt="Graph of available memory wait on the Storage service over the same 30 day period.  Available memory was constant" src="images/store_memory.png">

----

## Ingest boxes

- Look for high I/O wait
- Look for peaks in SAR NFS
- Look for any significant drops in available memory or high swap
  - Note trends in memory before and after patching

----

Ingest Server - CPU 

<img alt="Graph of CPU usage by Ingest service over the same 30 day (Aug 11 to Sep 10) period with a peak in early Sept" src="images/ingest_cpu.png">

----

Ingest Server - IO wait

<img alt="Graph of IO wait on the Ingest service over the same 30 day period with a peak in early Sept" src="images/ingest_sar.png">

----

Ingest Server - NFS wait

<img alt="Graph of NFS wait on the Ingest service over the same 30 day period with a peak in early Sept" src="images/ingest_sar_nfs.png">

----

Ingest Server - Memory Headroom

<img alt="Graph of available memory wait on the Storage service over the same 30 day period.  Available memory shows a gradual decline which is likely to be leveling off" src="images/ingest_ram_headroom.png">


----

## Audit Service

- Continually processes content
- Performs fixity check on cloud content every 60 days

----

## Audit - constant load

<img alt="Graph of audit service CPU over the same 30 day period.  The graph consistently high CPU usage" src="images/audit.png">

----

## Learnings - Capacity Planning

----

## Learnings - What to be aware of in each review
- server patching
- software releases
- known service downtimes

----

## Learnings - Issues Resolved

- IO Wait revealed
  - need for IO-optimized instance types
  - replace AWS EFS with ZFS for better throughput

----

## Leanings - Continue to Watch

- RAM - Available headroom
  - Memory Leaks
- Unreleased database connections

----

## Discussion

- How do your teams review server performance?
- How do your teams perform capacity planning?

---

## Software Dependency Checks

- Vulnerabilities in Code Repos
- End of life for Code Frameworks
- Obsolete versions of 3rd Party software

----

## Software Dependency Review

- The actual script resides in a private repository.  
- A [sanitized version](https://github.com/CDLUC3/uc3-present/blob/main/monthly_ops/dependency_scans.md) is provided here.
  - _some non-public links have been obscured_

----

## Code Repo Vulnerabilities

- Review
- Evaluate severity
- Assign tickets to resolve

----

## Review Feature Library Versions

- MySQL
- ZooKeeper
- Apache Tika

----

## Review Language Version and End of Life

- Ruby
- Java

----

## Review Framework Versions and End of Life

- Rails
- Tomcat
- jQuery

----

## Review Build Tool Versions 

- Apache Maven

----

## Review Build Tool Plugin Versions

- Maven plugins

----

## Set Schedule for Next Review

----

## Identify General Purpose Libraries with New Versions

- we have identified a tool
- we have not yet implemented this
- we just completed a large migration

----

## Learnings Dependency Review

----

## Learnings

- The information we needed was available online
  - reference sites to search
  - services/sites that push alerts to us
- Some end of life dates have surprised us

----

## Learnings

- Some checks are OK to do quarterly or biannually
  - we should note when the next review should occur

----

## Learnings

- We have been rewarded by every effort we have made to 
  - normalize builds
  - consolidate dependency configuration
  - automate testing

----

## Discussion

- How do your teams review software vulnerabilities?
- How do your teams keep 3rd party software up to date?

---

## Error Logs

----

## Consolidated Error Logs

- We consolidated all of our logs in OpenSearch in 2023

----

## Consolidated Logs

- Expedites investigation of a user-reported problem

----

## Consolidated Logs

- Enables us to discover errors before they become user-reported problems

----

## Error Discovery

- We rely on 
  - OpenSearch Saved Searches
  - OpenSearch Visualizations
  - OpenSearch Dashboards
  - Constantly refining our focus

----

## Dashboard Review

- The actual script resides in a private repository.  
- A [copy](https://github.com/CDLUC3/uc3-present/blob/main/monthly_ops/dashboard_review.md) is provided here.


----

## Web Application Firewall (WAF) Logs

- Make note of timeframes where errors may have been introduced by malicious activity
- Identify categories of malicious activity

----

WAF Logs

<img alt="Animated gif illustrating opensearch filters on Web Application Firewall Logs.  First, filter for blocked traffic.  Second, isolate to a peak period.  Third, focus on PHP-related blocked requests.  Note sample request paths." src="images/waf.gif">

----

## Learnings - WAF

- Anticpate random site crawling each month
- Rate limit attempts to login to the site
- Limit resource-intensive queries for unauthenticated users

----

## Application Logs - User Interface

----

## User Interface Errors by return code

<img alt="Dashboard containing Aggregate totals of Merritt UI return codes grouped by Irregular and Regular responses along with an explanation of each return code" src="images/return_code_dashboard.png">

----

Exploring User Interface Errors

<img alt="Animated gif illustrating an opensearch dashboard showing Merritt UI errors.  The user drills down to messages with a 500 return code that have been classified as 'atom' requests.  User browse one message in detail." src="images/ui-dashboard.gif">

----

## Learnings - UI Errors

- Expect 401/404 Errors
- Explore/Prevent 500 Errors

----

## UI 500 Errors

- Stale database connections
  - Retry logic
  - Reset connections

----

## Backend Service Errors

----

Storage Service Errors

<img alt="Date histogram of storage errors over the last 30 days showing 42 errors in the early part of the month" src="images/storage_errors.png">

----

## Capacity/Performance Analysis Using Log Data 

----

Bytes processed, computed from application logs

<img alt="Dashboard illustrating the Cumulative Total of the bytes processed by the Merritt Storage and Access services over a 3 hour period" src="images/assemblies.png">

----

## Leanings Capacity Analysis

- Partner organization was requesting **18T of data assembly per week**
  - Surprise to us and our partner
- We began queueing "large" requests

----

## Discussion

- How do your teams handle error logs?
- Do you have a process to discover unreported errors?

---

## Our Review Meeting Process

- Review
- Document action items as tickets
- Refine script for the next meeting

----

## Meetings

- Initially, we ran out of time at each meeting
- Eventually, we can cover the script in half of the time
- End early
- OR identify how to go deeper

----

## Meetings

- An additional meeting is an interruption
- Generally, we are glad we did it by the end
- It is more fun when something went wrong during the month

----

## Team Member Feedback

- Connects team members to Security 
- Connects team members to Cost Information
  - Cost review is handled by our DevOps engineer
- Greater appreciation of what our logs can do

----

## Beyond Our Team

- Our manager appreciates the ability to dive into details with the team
- Folks beyond our team have joined  us to observe the process
  - complimentary responses
- This has given the initiative a nice boost

----

## Script

- Script contains collective learning
  - Markdown is easy to edit
- Regular review builds confidence
- We challenge ourselves to go deeper as we have solved the immediate issues

----

## Discussion

- Do these ideas sound applicable to your environments?


---

## Where to go next

----

## Capacity Planning

- Covered pretty well at the server level
- Key metric computations could be interesting

----

## Software dependencies

- Library updates unrelated to vulnerabilities

----

## Error Logs

- What content is missing from our log files
- What index keys are needed
- What visualizations are need

----

## Fun Questions to Answer

- What system functions require the most retries?
- Can we graph periods of time where retries increase?
- Can we graph retries required by cloud provider?
- Can we quantify throughput by cloud provider?

---

## Thank You
- https://github.com/terrywbrady
  - UCTech Slack: Terry Brady (UCOP-CDL)
