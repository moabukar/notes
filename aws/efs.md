# EFS

- EFS is basically NFSv4. 
- EFS file systems can be mounted in Linux servers. 
- EFS file systems can be shared between many EC2 instances
- Private service, acces to file systems is via mount targets inside a VPC. Isolated to the VPC it's provisioned in. Even though it's private, you can access them via hybrid networking methods like VPC peering, Direct Connect, or VPN.
- EFS operates like a traditonal file system and adheres to POSIX permissions like Linux file systems do. like read, write, execute, and so on.

## Architecture

- An EFS file system is made available inside a VPC via mount targets and these run from subnets inside the VPC
- The mount targets have IP addresses taken frm the IP address range of the subnet they're in. To ensure HA, put mount targets in multiple AZs.
- Just like NAT GWs, for a fully HA system you need to have a mount target in every AZ that a VPC uses.
- It's these mount targets that instances use to connect to the EFS file systems.
- EFS is only for Linux only. 2 classes, standard & infrequent access. You also have lifecycle policies. 
- 
