# Bastion hosts aka Jumpbox

- Bastion hosts referred to as jumpboxes slightly incorrectly
- Positioned at the edge of your network to provider secure incoming access to your network
- Used by sys admins and engineers to access secure or sensitive networks remotely

Bastion hosts
- A server which runs at a network edge
- ...hardened to withstand attacks from public networks
- Acts as a gatekeeper between 2 zones (public internet => private network such as VPC)
- A jumpbox 
  - A server which runs between two or more different networking zones (as name suggest it allows you to jump between networks using this server)
- A bastion is a subset of jumpbox ...hardened (specifically runs between a public network and a private one)
  - Imagine a person on a public network who needs to gain accesss from the public network into a private and much more secure sensitive network
  - In the case of AWS, from the public internet into a VPC. 
  - Essentially used as an ingress control point. A single hole in your perimeter network which is extensively logged, has some form of authentication either using SSH keys or some kind of ID federation (so you can use corp creds to access it) and tends to have tightly controlled network security to restrict who can connect to it and from where.

Architecurally:
- Bastion host sits between the VPC ad public internet.
- Users connect to the bastion host and then from there, they can connect to the private network. Using SSH keys and can even use authentication forwarding
- VPC network protected via hardened bastion at perimeter 

- You can use systems manager to connect into private instances using IAM Authentication and use ssm to do port forwards.
- Bastion hosts are vendor neutral and can be used in any cloud provider
