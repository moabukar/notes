# AWS

## API GW vs ALB

- Can implement rate limiting on API GW but not ALB
- Both can integrate with WAF for protection
- API GW accepts HTTPS traffic only, ALB can accept HTTP and HTTPS
- API GW able to do request validation, request-response mapping, ALB cannot
- API GW can export/import cross API platforms using swagger, Open API 3.0, ALB cannot
- API GW can do caching, ALB cannot
- API GW time out is 30 seconds, ALB is 4000 seconds
- API GW integrates with almost all AWS services, ALB only integrates with EC2, Lambda, IP targets
- API GW can't do health checks, ALB can
- API GW pay per use unless you have caching enable, ALB pay per use (underlying EC2 instances)
- Price is not straightforward, depends on the number of requests, data transfer, caching, etc.
  - If your workload is spiky and idle for a long time, API GW is probably cheaper
  - But if your workload is consistently using high volume, ALB is probably cheaper

## Scaling:

- Vertical scaling: increase the size of the instance
- Horizontal scaling: increase the number of instances (better and more recommended for larger distributed systems)

### Lambda scaling:

- Lambda scales out (not up) automatically
- Lambda functions are independent, 1 event = 1 function
- Lambda is serverless, no servers to manage

### Container scaling:

- ECS scales out (not up) automatically.
- ECS can be integrated with EC2 Auto Scaling groups, which can scale out (not up) automatically
- ECS cluster can scale out (not up) automatically using CloudWatch Alarms



### EKS scaling:

- Cluster Autoscaler: scales the number of nodes within a cluster
- Horizontal Pod Autoscaler: scales the number of pods within a deployment/VM/EC2 instance

### Fargate scaling:

- AWS manages the scaling of the nodes for you (no underly EC2/server instances to manage)
- No Cluster Autoscaler here because AWS manages the scaling of the nodes for you

## Question:

- How can you make your application scaalable for a big traffic day?

Average answer: Put VMs in an Auto scaling group behind an elastic load balancer

## Answer:

- Put VMs in an Auto scaling group behind an elastic load balancer
- On a big traffic day, the burst will be high - so you can pre-warm your load balancers to ensure that they can handle the load
- You can also pre-warm your auto scaling group by setting a minimum number of instances to start with
- Also utilise shecudled scaling to increase the number of instances before the big traffic day (like in the morning at 6am)
- Ensure the AMI is lightweight (so it can boot up quickly & no unnecessary software and packages)
- If my application is connecting to a database, I will use a database proxy to handle the connections (so that the database is not overwhelmed with connections) like RDS proxy as it will maintain the pool of connections to the database
- Run IEM (Infrastructure Event Management) to monitor the health of the application and the underlying infrastructure to ensure it can handle the load
- Possibly increase the limit quota of the service (like EC2 instances, load balancers, etc.) before the big traffic day
- Also, talk about breaking the app into micro services


### Serverless scaling

- Ensure provisioned concurrency is enabled (will pre warm the lambdas so it can handle the burst of traffic)
- Optimise Lamdbda code using X-Ray to identify bottlenecks
- Optimise lambda configuration using CloudWatch insights to identify bottlenecks
- If using API GW, enable API caching
  - Use HTTP API instead of REST API (as HTTP API is faster & cheaper)
- Increase account limits (utilise different combo of account & region)
- If using DBs, Use a database proxy (like RDS proxy) to handle the connections to the database


### K8s scaling

- Use HPA (Horizontal Pod Autoscaler) to scale the number of pods within a deployment/VM/EC2 instance
- Use replica set to run multiple copies of pod & horizontal autoscaler
- Use cluster overprovisioner to provision new nodes
- If using ALB ingress, pre-warm the ALBs
- Use database proxy (like RDS proxy) to handle the connections to databases
- Increase account limits early

## Question:

How can you divert traffic for a specific domain (www.xyz.com) to a particular ALB? 

- Create an "A" record in Route 53 and divert any traffic for www.xyz.com to the ALB
