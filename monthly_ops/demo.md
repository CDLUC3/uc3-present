## Adopting Monthly Operational Review Meetings as a Learning Exercise

https://github.com/terrywbrady

---

## Presentation Purpose

Staying on top of security threats and operational issues can be an overwhelming challenge and a tedious challenge.

----

## Outline

- Challenge: development team needs to track operational issues
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
- 1 DevOps engineer (supporting multiple teams)

----

## Our Backgrounds

- Primarily software development (Java, Ruby, SQL)
- One developer has a Systems Admin background
- Our DevOps engineer has a Systems Admin background
- CDL has 3 full-time Systems Administrators supporting multiple teams

----

## DevOps Adoption at CDL

- We are adopting a DevOps approach
- Need to empower developers to perform operational functions
  - without turning them into Systems Administrators

----

## Immediate Needs for the Develpment Team to Take On

- Capacity Planning
- Software End of Life
- Software Dependencies
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

## Our Process

- Schedule time once a month
- Limit to 30 minutes
- Accomplish as much as we can
- Figure it out together
- Document learnings
- Document a script the next session

----

## Areas of Focus

- Capacity Planning - Server Performance
- Software Dependency Review
  - Vulnerabilities
  - End of Life
- Proactive Error Log Tracking

---

## Capacity Planning - Server Performance

----

## Our Meeting Script

- The actual script resides in a private repository.  
- A [sanitized version](https://github.com/CDLUC3/uc3-present/blob/main/monthly_ops/routine_librato_checks.md) is provided here.

----

<img height="80%" alt="Screen Shot of Capacity Planning Script used by the Merritt Team" src="images/cpscript.png">

----

## Store/Ingest - subject to peak processing

- Ingest service: download and validate
- Storage service: upload to S3 and validate

----

## Review Monthly Stats

- Identify peak processing
----

<img height="80%" alt="Bytes ingested over a 30 day period with a significant peak on Sept 05" src="images/bytes.png">

----

## Server Notes

- Link to performance charts
- Expected performance
- Key items to review

----

## Sample Performance Charts

----

## Storage Service

- Look for sustained CPU near 100%
- Look for peaks in SAR NFS
- Look for any significant drops in available memory or high swap

----

Storage Server - CPU

<img  height="80%" alt="Graph of CPU usage by Storage service over the same 30 day (Aug 11 to Sep 10) period with a peak in early Sept" src="images/store_cpu.png">

----

Storage Server - IO Wait

<img height="80%" alt="Graph of IO wait on the Storage service over the same 30 day period with a peak in early Sept" src="images/store_sar-io.png">

----

Storage Server - NFS wait

<img height="80%" alt="Graph of NFS wait on the Storage service over the same 30 day period with a peak in early Sept" src="images/store_sar-nfs.png">

----

Storage Server - Memory Headroom

<img height="80%" alt="Graph of available memory wait on the Storage service over the same 30 day period.  Available memory was constant" src="images/store_memory.png">

----

## Ingest boxes

- Look for high I/O wait
- Look for peaks in SAR NFS
- Look for any significant drops in available memory or high swap
  - Note trends in memory before and after patching

----

Ingest Server - CPU 

<img height="80%" alt="Graph of CPU usage by Ingest service over the same 30 day (Aug 11 to Sep 10) period with a peak in early Sept" src="images/ingest_cpu.png">

----

Ingest Server - IO wait

<img height="80%" alt="Graph of IO wait on the Ingest service over the same 30 day period with a peak in early Sept" src="images/ingest_sar.png">

----

Ingest Server - NFS wait

<img height="80%" alt="Graph of NFS wait on the Ingest service over the same 30 day period with a peak in early Sept" src="images/ingest_sar_nfs.png">

----

Ingest Server - Memory Headroom

<img height="80%" alt="Graph of available memory wait on the Storage service over the same 30 day period.  Available memory shows a gradual decline which is likely to be levelling off" src="images/ingest_ram_headroom.png">

----

## Database

----

## Audit - constant load

----

## Query Performance

----

## Learnings

- What to ignore
  - server patching
  - software releases
  - known servcie downtimes
- RAM - Available headoom

---

## Software Dependency Review

----

## Code Repo Vulnerabilities

- Review
- Evaluate severity

----

## Create Tickets

----

## Review Feature Library Versions

- Database
- Queue
- Apache Tika

----

## Review Language Version and End of Life

----

## Review Framework Versions and End of Life

----

## Review Build Tool Versions 

- Apache Maven

----

## Review Build Tool Plugin Versions

----

## Set Schedule for Next Review

----

## Identify General Purpose Libraries with New Versions

---

## Error Logs

----

## Consolidated Error Logs

----

## Dashboard Review

----

## WAF Logs

- Web Application Firewall

----

## User Interface Errors

----

## Backend Service Errors

----

## Future Growth

- Capacity analysis using application logs

---

## Process

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

## Script

- Script contains collecttive learning
- Regular review builds confidence

---

## Where to go next

----

## Capacity Planning

- Covered pretty well

----

## Software dependencies

- Library updates unrelated to vulnerabilities

----

## Error Logs

- What content is missing from our log files
- What index keys are needed
- What visualizations are needd

---

## Discussion