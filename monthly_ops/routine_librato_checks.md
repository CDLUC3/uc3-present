  ## Content ingests - last 30 days
  - [Chart of Content Ingest Bytes over the Last 30 days](#)
  - [OpenSearch View](#)
  
  ## Routine Librato Checks (last 4 weeks) to perform at each Sprint Review
  - [RDS](#)
    - expect steady CPU below 50%
    - examine available memory and recovery from drops
    - expect steady number of connections - review any abnormal spikes
    - NEW! review main account RDS Performance Report
    > We've added a new role that you can access in the AWS Console: your AWS SSO screen should show a new entry for the "cdl-main" account; expanding that you'll see the "cdl-main-devops" role. Go in there, then navigate to RDS -> Performance Insights, and pick your database. 
  - [Store](#)
    - Look for sustained CPU near 100%
    - Look for peaks in SAR NFS
    - Look for any significant drops in available memory or high swap
    - [Store ALB](#)
      - track 400/500 errors from the app 
      - Watch for healthy/unhealthy hosts
  - [Ingest](#)
    - Look for high I/O wait
    - Look for peaks in SAR NFS
    - Look for any significant drops in available memory  or high swap
      - Note trends in memory before and after patching 
    - [Ingest ALB](#)
      - Watch for healthy/unhealthy hosts... note how this may correlate to ZFS maintenance 
  - [Audit](#)
    - Expect CPU to be consistently near 100%
    - Expect low SAR values
    - Expect steady available headroom 
  - [Replic](#)
    - Expect steady available headroom
    - TBD 
  - [UI](#)
    - Expect consistently low CPU
    - Expect consistent memory headroom 
    - [UI ALB](#)
      - Track number of requests
      - Track 400/500 errors from the app 
      - Track 400/500 errors from the load balancer (WAF)
      - Note spikes in latency
  - [Access](#) 
    - ~Expect CPU to be consistently near 100%~
    - Expect steady available headroom 
    - [Access ALB](#)
      - Watch for healthy/unhealthy hosts
  - [Inventory](#)
    - Expect consistently low CPU
    - Expect steady available headroom 
    - [Inventory ALB](#)
      - Watch for healthy/unhealthy hosts
  - [ZK](#) 
  - [S3](#)
    - explore behavior by bucket: 1001 - assembly, 5001 - s3, 6001 - glacier
    - examine 400 errors
    - note the unique behavior of the 1001 Assembly bucket

## Cloudwatch Checks
  - [ZFS](#)
    - TBD