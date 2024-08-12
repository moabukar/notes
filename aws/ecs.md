# ECS

- ECS is to containers what EC2 is to VMs
- ECS has 2 modes: EC2 mode which uses EC2 instances to run containers and Fargate mode which is a serverless mode where you don't need to provision EC2 instances to run containers.
- Container definition: Images & port
- Task definition: Security (task role), container(s) & resources
- Service: Number of tasks (horizontal scaling), load balancer, network config, min & max healthy percent, restarts. You can put an LB in front of this to distribute traffic between tasks.
- Cluster: Group of EC2 instances or Fargate tasks
- Task role (IAM role which the TASK assume)

## ECS vs Fargate

- EC2: ECS provisison these EC2 container hosts but there is an expecatation that youw will manage them.
- Fargate: ECS provisions the containers for you. You don't need to manage the underlying infrastructure.
  - Fargate uses a shared infrastructure which is abstracted away from you and AWS manages this.
 
### EC2 vs ECS (EC2) vs Fargate

- If you use containers, use ECS
- Large workload - price conscious: use EC2
- Large workload - overhead conscious: use Fargate
- Small/Burst workload: use Fargate
- Batch/Periodic workload: use Fargate
