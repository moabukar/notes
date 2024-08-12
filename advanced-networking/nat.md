# NAT

## NAT instances

- Normal EC2 instance running an AMI configured to provide NAT services
  - no longer recommened because it's running an Amazon Linux, 2018.03 AMI (end of life)
  - AWS recommends customers to move to NAT Gateway
  - non-specialised, just EC2 instance running NAT software
- Speed based on size and type of EC2 instance
- Disable source/dest check (if interface is neither the source nor destination then by default it will drop the packets) (so enable this option if you're using NAT instances)
- SGs on the NAT instance & NACL on the subnet
- Can also be used as a bastion host and can be configured for port forwarding
- ...self managed, so you have to patch it, monitor it, etc.
- Scripts are used to implement HA
  - they are just EC2 instances so failures need to be managed with thes scripts
  - route table changes based on failures
- Using NAT instances can help reduce costs as you can combine different functionalities
  - combine NAT instance with a bastion host and port forwards all running from the same EC2 instance
- SG & NACL can be used to control access to the NAT instance
- Flows logs for monitoring - to review packet metadata that's flowing through the VPC. You could put a VPC flow log to specifically monitor the network interface of the NAT instance or scope it wider by putting it on the subnet or VPC
- NAT instance can be used across DX, VPN, peer connections (not ideal but possible)
 
## NAT Gateway (AWS Recommended)

- You have 2 subnets (one private and one publi)
  - The private subnet is not public routable as it has private IPs like 10.16.32.0/20 (app subnet)
  - The public subnet is public routable as it has a route table attached with a route to the internet gateway (IGW). The public note allows us to use public addressing. 
  - We could grant the private subnet access to the internet by provisioning a NAT gateway to the public subnet. Because the public subnet allow us to use public addressing, the public subnet has a route attached to it which provides default IPv4 routes pointing at the IGW. So the NAT GW is allocated with a public IP which is routable across the public internet, it can send data out and get response back in return
  - We need to understand here that this public IP address which the NAT GW has allocated is NOT really public. An IPv5 within a VPC, nothing really has a public IP, it's allocated with a public IP which is associated with its private IP and the IGW translates between the two. 
  - So in this case, the NAT GW actually has a private IP which is mapped onto a public IP by the internet gateway.
  - The private subnets where the instances are located, they have their own route tables which are different to the public subnet route tables. These route tables have also a default IPv4 route which points to the NAT GW. So the instances in the private subnet can send data to the NAT GW which then sends it out to the internet gateway which accesses the internt and gets a response back.

Example flow:
  - Instance 01 sends some data. It has a source IP and dest IP. The source IP is the EC2 instance private IP and the dest is the dest IP 1.3.3.7
  - Because of the default route on the route table, the packet is routed to the NAT GW
  - The NAT GW receives the packet and it makes a record of hte data packet, it stores the destination that the packet is for and the source IP of the EC2 instance and all details such as ports IP
  - then the NAT GW adjusts the source IP of the packet to be its own IP address and dest IP stays the same (1.3.3.7). The NAT GW IP is still a private IP. The NAT GW is in a public subnet and the subnet has a route table which has a default route pointing to the IGW. The NAT GW has a private IP but the IGW does 1:1 NAT and maps the private IP to a public IP. So the NAT GW has a private IP but the IGW translates it to a public IP.
  - Then the packet is moved on towards the IGW by the VPC router. At this point, the IGW knows that the packet is from the NAT GW. It knows the NAT GW has a public IPv4 associated with it, so it modifies the packet to have a source IP as the NAT GW public address and it sends it out to the internet
  - The NAT GWs job is to allow multiple private IPs to masquerade behind the IP address that it has. That's where the term IP masquerading comes from. It's a form of NAT.
  - The IGWs job is to translate a private IP into a corresponding public IP which is associated with that private one.
  - So the NGW & IGW work together to allow private IPs to access the internet and get a response back.
  - If you need to give instances public IPv4 directly, then only the IGW is required. If you want to give private instances outgoing access to the internet, and AWS public zones such as S3 then both the IGW and NAT appliances are required (either a NAT instance or a NAT GW).

### NAT GW Advanced

- Runs from a public subnet & uses elastic IP (static IPV4)
  - so to deploy a NAT GW, you already need your VPC in a position where you have a public subnet and for that you need an IGW, subnets configured to allocated public IPv4 addresses and default routes for the subnets pointing to the IGW
- NAT GWs are AZ resilient service (HA in that AZ) - hardware failure 
  - for region resilience, NAT GW in each AZ
  - Route table for each AZ with that NAW GW as target
- To implement a truly regional resilient architecture  for NAT services within a VPC, you need a NAT GW in a public subnet in each AZ that the VPC uses
  - Then as a minimum, you need private route tables in each AZ 
- Common mistake: that 1 NAT GW is enough for a VPC to provide region resilience, in other words a NAT GW is truly HA. It's NOT and this is FALSE. 
  - A NAT GW is HA within the AZ that it's running from. So if hardware fails or if it needs to scale to cope with load, then it can do so within that 1 AZ. But if the whole AZ fails, then there's no failover. 
  - You need to provision a NAT GW into a specific AZ, not the region. It's not like the IGW which is by default resilient across all AZs in a region.
- 5Gbps by default, can scale up to 45Gbps based on load
  - If you need more, split resources into multiple subnets and use multiple NAT GWs
- Elastic IP cannot be changed on a NAT GW
  - If you need to change the private IP of the NAT GW, you need to delete and create a new NAT GW
- NAT GW cannot be used across VPC peers, S2S VPN, DX
  - Only within the VPC
  - But, NAT instances can be used across VPC peers, S2S VPN, DX
- 55,000 simultaneous connections per NAT GW to each unique destination IP
  - ~ roughly 900 connections per second to a single destination IP (55,000 connections per minute)
  - If you reach this level, you risk issues with port allocation on the NAT GW which leads to port allocation error (uses 1024-65535) >> viewed in CloudWatch by looking for ErrorPortAllocation metric
  - If you need to break through these limits, you need to split your resources into multiple subnets and use multiple NAT GWs
- For S3, DynamoDB, use gateway endpoints in your VPC (as it's free)
- You secure NAT GWs by using NACLs on the subnets and SGs on the private instances
  - so either the the source subnets or the NAT GW subnets
- You CAN'T use SGs on the NAT GWs but you can on NAT instances

