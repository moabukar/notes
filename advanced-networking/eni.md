# ENIs

- Basics covered ..



## More on ENIs

- You cannot detach a primary ENI from an instance (you can only detach secondary ENIs)
- You associate SGs with ENIs and not with individual IPs
- 2nd ENI allows instance to be multi-home (subnets) in same AZ
- ENIs cannot be used for NIC teaming which means they cannot be used together to increase instance network bandwidth
  - adding multiple ENIs does not add that much bandwidth
- The number of ENIs that you can attach to instance and number of secondary IP addresses per ENI depends on EC2 instance type
  - For example, if you are using EKS and it uses VPC CNI plugin - how many pods you can launch on a given EC2 instance depends on how many private IPs you have. So if you take c4.xlarge as the instance, you can have 4 ENIs and each ENI can have 15 private IPs. So you can have 60 pods on that instance. [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html]

- Cross-account network interface
- 


  
Dual-homed instances
- You can have 1 instance that uses 2 ENIs. 1 ENI to access the public internet via an IGW
- The other ENI can be used to access a private network such as a Corporate data center via virtual private gateway or direct connect etc

