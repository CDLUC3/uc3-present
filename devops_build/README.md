## On the Journey to DevOps, Start with Your Build

---

- Notes
    - Re-use from the builds presentation
    - Re-use from the ECS presentation 

----

- Merritt system
    - Amount of content
    - Deposit extremes

----

- Merritt current system 
    - Server count
    - ALB count

----

- AWS migration in ??
    - Lift and shift
    - Managed EC2 instances

----

- Cloud state
    - S3
    - ALB, WAF
    - Lambda
    - SSM Parameters
    - OpenSearch

----

- Staffing
    - PM
    - Dev
    - DevOps
    - Support from Sysadmins

----

- Current devops
    - DevOps support of Puppet and Ansible resources
    - Not core skills of the majority of them
    - AWS permissions granted to EC2 instances
    - Very limited console access
    - SysAdmins implement AWS changes

----

- Devs
    - Excited by cloud technologies
    - Lots to learn
    - Want more control
    - Hesitant about becoming SysAdmins

----

- Direction
    - Infrastructure as code
    - Disposable resources (rather than patching persistent assets)
    - Service limitation - need to scale
    - Persistent resources make this difficult

----

- Migration plan
    - Multi-year migration
    - Resources moving from centralized account to program specific accounts
    - Dev teams will have console access 
    - Everything in production will be created with infrastructure as code

---

- Start simply
    - Colleague was publishing some resources to CloudFront
        - I had thought of Cloud Front as an expensive solution
        - Learned it was a preferred way of publishing assets
    - Very handy for Merritt
    - Merritt Dev Resources

----

- Dev resources
    - JavaDoc
    - RubyDoc
    - Cumbersome to keep up to date without a website

----

- Demo: Java Doc/Ruby Doc

----

- Code Build process
    - Git commit
    - Compile
    - Generate java doc
    - Copy to S3
    - Update CloudFront

----

- Demo: index page

----

- Next
    - Apply the same concept for publishing index page

----

- Great introduction to Sceptre and CloudFormation

---

- This progress Spiraled!

----

- Next
    - Generate javadocs for all Merritt libraries

----

- Next
    - Publish swagger documentation

----

- Next
    - Build java libraries and publish to artifact repo
        - AWS CodeArtifact

----

- Demo: library build

----

- Next
    - Build java services using published artifact resources

----

- Demo: Service build

----

- next
    - Build docker images used in integration testing of java services

----

- Next
    - Build all Merritt services at docker images

----

- Demo: image build

----

- Next
    - Schedule daily builds
    - Ensure up to date docker images (vulnerabilities)

----

- Demo
    - Schedule

----

- Next 
    - Run java end to end testing using Code Pipeline/CodeBuild

----

- Next
    - Schedule end to end testing every weekday

----

- Demo:
    - Daily end to end testing

----

- Replaced our legacy build system (Jenkins)
    - More feature rich
    - Just in time builds
    - Faster builds
    - More complete builds
    - End to end testing can be run by anyone - no environment configuration

----

- Team Response
    - Excited for the change
    - Appreciated the improvements
    - More engaged/interested in the DevOps migration effort

----

- By the end of this, I felt reasonably proficient with Sceptre

---

- Current effort
    - Building ECS stack (DEV)
    - Using those docker images
    - Auto-deploy to DEV stack at the end of the build

----

- Creation of admin tool
    - View state of app stack
    - View tags available as docker images
    - Tag specific images for deployment
    - Trigger deployment from the app

---

- Why the build?

----

- Build may be ripe for automation
    - Always more things that are useful to automate
    - In our case, we had end to end test script that we run manually after deployment/patching
    - This was a great excuse to automate it

----

- Separate from production
    - Great experimental platform
    - Easy to plug into a production system

----

- We appreciated the nuances of tagging
    - Library (jar and gem) tagging behaves differently than 
    - Deployed service tagging
    - Docker image tags behave differently from source code tags
    - Often useful to re-use the same tags!

---

## Thank You

