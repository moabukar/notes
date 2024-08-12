# High availability & Fault Tolerance

- System continues to operate even if some of its components fail
- System guarantees certain percentage of uptime

## Identifying single points of failure

- Servers running your applications
- Databases
- Load balancers
- Analyse each component and identify single points of failure


## Achieving high availability on AWS

- ELB > EC2 
- Run EC2s in an auto scaling group in multi-AZs
- Achieving HA costs money

- You can use Lambdas to achieve HA
- You can also achieve HA using EKS by using cluster autoscalers which deploy multiple nodes across multiple AZs

## HA vs Fault Tolerance

- In fault tolerant architecture not only is the system HA but performance does not degrade even when a component fails
- Fault tolerance is generally more expensive than HA systems
- High availability ensures system will keep serving traffic even if one availability zone goes down. Fault tolerant ensures the application can support same transactions per second even if one availability zone goes down
