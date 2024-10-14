## dloy

```
How work well + learning

These meetings provide important focus for understanding where and how things can go right and wrong in the functioning system.
Librato Review 
- provide better understanding how service functionality impacts hardware and cost resources. Many AWS credits become apparent.

Dependency Review 
- focus on external security issues. Many of these types of vulnerabilities are instructive for how external threats may access a system.
- encourages the development of processes to address overall system vulnerabilities. (e.g. BOM)

OpenSearch Log Review
- understandiing the importance of log mapping to track and debug. This feeds back to developing log entries that focus on specific low level operations that may be periodically failing or chewing resources.
```

## mreyes

```
The review meetings are helpful at many levels.
---------------------------------------------
Librato -
   a) Preemptively expose hardware issues before it affects services.
   b) Understatnding what the metrics mean and how changfes to AWS env impacts tem

Dependency
   a) Security.  Outdated software may have vulnerablilties

Opensearch
   b) Confirmation that services are working as expected

Learning
--------
   a) I've learned the most from Opensearch.  Centralized logging is very powerful and AWS allows for amazing search and filtering
   b) Showing Librato ALB (along with WAF) results has been a laerning experience as well

Changes
-------
   I'm not sure what could change.  The routine of going through these graphs and reports is a good way to refreshen what we track.
```

## mstrong

Marisa’s Perspective on the meetings
If you have a chance, it would be great to include your perceptions of these meetings
what has worked well with these meetings?
what have you learned from these meetings?
how would you recommend that these meetings change?

Librato Review
Librato Review - The graphs to review are very well organized with links to each librato graph.  Documented “normal” baseline information with reference to the Admin Tool to review recent activity and inform the effect of metrics presented. I like that the team has an overall awareness of the system performance health not just when things go awry.  This particularly helps when an issue arises and we understand what the norm should be and what to pay attention to and adjust accordingly, saving time and resources. I have gained a deeper understanding of the graphs and the information displayed.   
Dependency Review
I appreciate that they help with our security posture, knowing what vulnerabilities need to be addressed and having a shared conversation with the team about each vulnerability provides shared insight into our services.  As all of UCOP needs to have a security first philosophy, this practice helps us ensure compliance to security best practices.  
OpenSearch Log review. 
The OpenSearch Log review proved to be very beneficial to drill down to errors and understand what bugs may exist in the system and areas for improvement.  We’ve also been able to minimize traffic to our sites by having visibility into bots which impacts performance.  
The Libratro reviews allow our decisions around capacity planning and resource allocation to be data driven which can affect future operations of our services.
Regular reviews also help us to identify where we should be enabling alerts (zero db connections for example)
As someone who does not dive into the details on a daily basis I find them particularly helpful to understand the overall health of the system from a broad high level perspective.

Regular review also helps to identify where information is lacking; what would be nice to have that we don’t.  Requesting access to the AWS RDS Performance Insights is an example of this.  

Having a manual review is helpful but time consuming for all involved.  I appreciate the iterative approach to improve the process and document trends and specifics so the team as a whole benefits and can lead a review, not just one skilled individual. I would like to see others lead this practice.   I hope at some point the tools evolve to inform us more readily of anomalies and the process to be automated, perhaps reviewed on a less frequent basis.  They could be viewed as a tedious task but overall, seems to be well received by the team for the information it provides.  

