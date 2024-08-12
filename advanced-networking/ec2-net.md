# Advanced EC2 Networking

## EC2 & ENIs

- When dealing with EC2 networking, the most important part is the ENI (Elastic Network Interface) - they control how networking is managed for EC2 instances. Security groups and NACLs are attached to ENIs, and ENIs are attached to EC2 instances. 
- ENIs can be moved between EC2 instances. An EC2 instance can have multiple ENIs attached to it at the same time.
- The ability to attach multiple ENIs to a single instance is useful for various scenarios, such as:
  - Creating dual-homed instances: Instances with workloads or roles on different subnets or networks.
  - Managing traffic: Separating different types of traffic for security or performance reasons.
  - High Availability setups: Using secondary interfaces for failover or redundancy.
- EC2 instances have a primary ENI created with the instance which cannot be removed
- Addiitonal or Secondary ENIs can be added in and removed from other subnets (mult-homed) but not in other AZs

- Every ENI is allocated a single primary private IPv4 address which is obtained using DHCP. This IP address remains the same for the lifetime of the ENI, even if it is detached from the EC2 instance. 
- And so EC2 is always launched with a primary network interface, we know that every instance at the very least has one primary private IPv4 address which is static for the lifetime of the interface and thus it's static for the lifetime of the instance.
- So as a foundational facts, by default every EC2 instance has a single primary private IPv4 address that never changes. 
- Primary private IPv4 addresses, ENIs can also be allocated with one or more secondary private IPv4 addresses. 
- Note: IP addresses are linked to the ENI, not the instance.
- Security groups are associated with ENIs not with EC2 instances
- Each ENI can also be protected by a NACL around its subnet
- Most networking configuration which can influence how EC2 instances can be interacted with is performed at the interface level, not at the instance level. It's the network interfaces which have the SGs, it's the ENIs which are in subnets which have the NACLs.
- 

## Public IPv4 addressing

- If you launch an instance into a subnet which is set to allocated public IPv4 addresses, or if you explicity decide to launch an instance with a public IPv4 address, then it's allocated a public IPv4 address.
- Instead you it's better to have elastic IPs. and you can allocated 1 elastic IP per private IPv4 address. These can be moved between network interfaces and instances.
  - You get charged for elastic IPs when they are not attached to an instance or not being used.
- 

## Advanced EC2 Networking Architectures

- Management or isolated networks by using multiple ENIs.
- Network interfaces can also be used for Software Licensing (MAC)
  - Some legacy software is linked to the MAC address of a specific interface on a specific instance. So you can create a secondary network interface, attach to an instance, use the MAC address for the licensing. And if you need to migrate the software, you can detach the network interface from the instance, move it to another one, attach it and the software will still work because it's licensed based on MAC address which is linked to a specific network interface.
- Security or Network appliances
  - You can use network interfaces to create a security appliance or a network appliance. For example, you can create a network interface, attach it to an instance, install a firewall on that instance, and then attach that network interface to another instance. And now you have a firewall that's protecting the second instance.
- Dual/Multi-homed instances with workloads/roles on specific subnets. 
  - So if you had a mutli-tier app with a web server, an app server and a database server, you could have the webserver communicating with the app on one interface and the database server communicating with the same app server on another interface.
  - And on each interface you can have a different security group or NACLs to control the traffic.
- Low budget & simple HA solutions
  - Imagine you have 2 EC2 instances, which provider access to an app, you could have a secondary network interface, which is associated with one of those instances at any one time. That interface has an IP address, through this IP the server is provided, and if the instance fails, you could migrate that network interface to a different instance and fail over as part of a low budget HA solution.
  - So you have loads of flexibility by creating architectures using multiple network interfaces.
- Each interface has a flag associated with it known as source and dest checks. By default, if an interface processes traffic where it's not the source or the dest for that traffic, the packet is dropped. Disabling this check allows an ENI to process traffic that it hasn't created or it isn't the final destination for. Used for different types of network appliances like NAT instances. 
- So anytime you interact with an instance from a networking perspective, generally you're interacting with the primary network interface for that instance.
