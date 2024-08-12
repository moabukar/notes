# WAF

- Layer 7 application firewall. Can understand and filter traffic on anything layer 7 (HTTP, HTTPS, etc) . 
- WAF supports and protects web app based AWS services like CloudFront, ALB, AppSync, API GW.
- Web ACLs are associated with a resource like a CFront distribution and this ensures that the CFront distro is protected by WAF.
- WAF can protect global services like CFront and regional services like ALBs.
- Rules are within rule groups & Rule groups are within webACLs.
- WebACLs can be updated manually but you can also do simple automated updates to webACLs such as using EventBridge and scheduled rules to pass various publicly-maintained IP lists to block known bad actors
- WAF logs can be sent to S3, Kinesis Data Firehose, CloudWatch Logs, or CloudWatch Metrics. All of these locations can then be integrated with event-driven security response architecture. Such as S3 events, Lambda, Athena and EventBridge. You can extract and identigy intelligence to act on and to update web ACLs. 
- Architecture is based on creating a feedback loop between logs generated by WAF/Apps or ecosystem data to update WebACLs

## WebACLs 

- WebACL default action (ALLOW or BLOCK) - non matching
- Resource Type - CloudFront (global) or Regional service like ALB, API GW, App Sync
- You need to add rule groups or rules which are processed in order
- WebACL capacity units (WCU) - default 1500 but it can be increased with a support ticket
- WebACLs are associated with resources like CloudFront distributions, ALBs, API GW, AppSync
- Adjusting a webACL takes less time than associating one


## Rule groups

- Rule groups contain rules
- They dont have default actions, thats defined when groups or rules are added to webACLs
- Rule groups are either: managed by AWS/Marketplace or managed by you or service owned (Shield & Firewall Manager)
- Rule groups can be referenced by multiple webACLs
- A webACL can reference one or more rule groups

## WAF rules

- Type, Statement, Action
- Type: Regular or Rate-based
- Statement: (WHAT to match) or (COUNT ALL) or (WHAT & COUNT)
- Match against things like origin country, IP, label, header, cookies, query parameter, URI path, query string, body (first 8192 bytes only), HTTP method
- Rules can have multiple statements. And conditions like, Single, AND, OR, NOT
- Action: Allow*, Block, Count, Captcha, Custom Response (x-amzn-waf), label
- With rate-based rules, you only have block, count and captcha and there is no allow (since conceptually you want to do something if a rate is above a certain value)
- Labels can be referenced later in the same webACL (multi-stage flows)
- ALLOW & BLOCK stop processing, Count/Captcha actions continue

Basic Pricing:
- WebACL ($5/month or more per webACL) - they can be reused across different services like CFront, ALB, API GW, App Sync
- Rules on WebACL (1$ per month)
- Requests per webACL ($0.6 per month for every 1 million requests)
- Intelligent Threat Mitigation
- Bot control - ($10/month) & ($1 for every 1 million reqs)
- Captcha ($0.4 per 1000 challenge attempts)
- Fraud Control/Account Takeover ($10/ month & $1 per 1000 login attempts)
- Marketplace rule groups (extra costs)
 

# Shield

- Shield Standard (Free) & Advaned (DDoS protection) (costs)
- DDos attacks
  - Network volumetric attacks (L3) - Saturate capacity
  - Network protocol attacks (L4) - TCP SYN Flood ()
    - Generate a number of connections from a spoofed IP address
    - and leave connections open, prevent news ones - so never terminating them
    - Network protocol attacks can be combined with volumetric attacks (so L3 + L4)
  - Application layer attacks (L7) - like web request floods
- 

## Shield Standard

- Free & protection at the permiter (so at region/VPC or at the edge)
- Protects against common network and transport layer attacks (L3 or L4)
- You get best protection if you use Route 53, CloudFront and Global Accelerator

## Shield Advanced

- $3,000 per month (per org), 1 year lock-in + data (OUT)/m (so for 1 year, you pay $36,000 per month)
- Protects CF, R53, Global Accelerator, anything associated with EIPs (so like EC2, ALBs, NLBs and CLBs)
- Not automatic - must be explicitly enabled in shield advanced or AWS Firewall manager shield advanced policy
- Cost protection (EC2 scaling) for unmitigated attacks (if you as a customer incur any costs for any attacks which should be mitigated by shield advanced but aren't, then you are protected against those costs)
  - Example might be EC2 scaling events caused by excessive load
- Proactive engagement & AWS Shield Response Team (SRT)
- 

- WAF integration - includes basic AWS WAF feeds for web ACLs, rules and web requests 
- App layer L7 DDoS protection (uses WAF)
- Real time visibility of DDoS events and attacks
- Health-based detection - app specific health checks, used by proactive engagement team (to help reduce false positives detected by AWS Shield)
- Protection groups to create groupings of resources which Shield advanced protects
