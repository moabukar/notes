# EKS basics

- Managed K8s 
- Outposts, EKS Anywhere, EKS Distro
- Controlplane scales and runs on multiple AZs
- Integrates with AWS services such as ECR, ELB, IAM, VPC
- EKS cluster = EKS control plane + EKS worker nodes
- etcd is distributed across multiple AZs
- Nodes: self managed or managed node groups or Fargate pods
  - windows, GPU, outposts, local zones... check node type
- Storage providers: EBS, EFS, FSx for Lustre, FSx for NetApp ONTAP
- Control plane communicates with worker nodes via ENI which are injected into the customer VPC
- The kubelet service running on the worker nodes communicates with the control plane either via the ENI (which are injected into VPCs - kube-api traffic ) or a public control plane endpoint. Any admin via the control plane can also be done using this public endpoint. EKS admin via public endpoint.
- Any consumption of the EKS serviecs is via ingress configurations which start from the customer VPC. 

# Common challenges faced

## Run out of IP address spaces & latency

EKS clusters can run out of IP addresses for pods when they reach between 400 & 500 pods. With the default CNI settings, each node can request more IP addresses than is required.

- Use prefix assignment
- Add secondary CIDR blocks
- Use overlapping non routable CIDR blocks (different regions?)
- Use IPv6 (but what are the challenges of these?)

Advanced suggestions:

To ensure that you don’t run out of IP addresses, there are two solutions:

- Set MINIMUM_IP_TARGET and WARM_IP_TARGET instead of the default setting of WARM_ENI_TARGET=1. The values of these settings will depend on your instance type, expected pod density, and workload. More info about these CNI settings can be found here. The maximum number of IP addresses per node (and thus maximum number of pods per node) depends on instance type and can be looked up here.

- If you have found the right CNI settings as described above, the subnets created by eksctl still do not provide enough addresses (by default eksctl creates a “/19” subnet for each nodegroup, which contains ~8.1k addresses). You can configure CNI to take addresses from (larger) subnets that you create. For example, you could create a few “/16” subnets, which contain ~65k IP addresses per subnet. You should implement this option after you have configured the CNI settings as described in #1. To configure your pods to use IP addresses from larger manually-created subnets, use CNI custom networking (see below for more information)

## Cluster Autoscaler is slow during traffic spikes

- Use AWS Karpenter 
  - More performant than Cluster Autoscaler
  - Spins up in seconds compared to minutes of cluster autoscaler
  - it can directly access the AWS API to spin up instances
- Use cluster overprovisioning
  - Launch dummy pods with negative priority to reserve EC2 capacity
  - Once the higher priority jobs are scheduled, these pause pods are preempted to make room for high priority pods which in turn scales out additional capacity as a buffer
- Scale CoreDNS with proportional autoscaler
  - CoreDNS will scale based on the number of ec2 worker nodes

Advanced suggestions:

Configure overprovisioning with Cluster Autoscaler for higher priority jobs

- If the required resources is not available in the cluster, pods go into pending state. Cluster Autoscaler uses this metric to scale out the cluster and this activity can be time-consuming (several minutes) for higher priority jobs. In order to minimize time required for scaling, we recommend overprovisioning resources. You can launch pause pods(dummy workloads which sleeps until it receives SIGINT or SIGTERM) with negative priority to reserve EC2 capacity. Once the higher priority jobs are scheduled, these pause pods are preempted to make room for high priority pods which in turn scales out additional capacity as a buffer. You need to be aware that this is a trade-off as it adds slightly higher cost while minimizing scheduling latency. You can read more about over provisioning best practice here.


## FluentD has more plugins, but FluentBit is faster

- Better to switch to fluentBit
- FluentBit can put logs to Kinesis Data Firshose 


## Cost


###################### New Section ######################

## Policy Enforcement using Admission Controllers

- You can implement tools like OPA Gatekeeper to enforce policies on your cluster or Kyverno


## Cross AZ Traffic

- Traffic in same AZ is free but cross AZ traffic is charged
- ALB egress traffic is also charged but minimal
- Each node has an AZ specific label like `topology.kubernetes.io/zone=us-east-1a`
  - You can use this label to schedule pods to specific AZs (`nodeSelector` & `nodeAffinity`)
- With cluster autoscaler, create managed node groups in same AZ. 
- With Karpenter, define specific AZs in the provisioner
- Utilize topology aware hints (K8s upstream v1.24 and beyond)


## K8s Design - microservices

- Ingress ALB acts as the face of the microservice

## K8s Design - batch job

## K8s Design - event driven

## K8s DR

- Replicate your architecture (EKS, RDS, subnets) in another region
- You can use R53 to do failover routing in case one region goes down
- You can also use AWS Global Accelerator to do faster failover routing
- You need to do RDS Read Replicas in the DR region

- DR stratgies
  - Warm standby is an acce
    - RPO/RTO: few minutes


## K8s cost

- Costs of K8s are divided into Control plan & data plane and other

- Control plane costs:
  - EKS (fixed)

- Data plane costs:
  - Workload (pods/containers)/Node/Fargate
  - Autoscaling
  - Network costs
    - Pod data transfer across AZs
    - ECR
    - ELB
    - NAT GW & more
  - Observability costs
    - Logging & monitoring (Cloudwatch, Prometheus, Grafana)
    - Running agents

  
- 70% of costs goes to workload

- How to optimise costs:
  - K8s pod request/limits optimisation
    - Sometimes devs give way too much resources to pods
    - Use something like kubecosts to get insights into pod resource usage

- How to create cluster with EKS best practices
  - EKS blueprints
