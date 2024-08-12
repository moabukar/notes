# Extending VPC address space

- If you have exhausted the address space of your VPC, you can extend it by adding secondary CIDR blocks to your VPC.
- CIDR block must not overlap with existing CIDR block or peer VPC CIDR block.
- If primary CIDR is from RFC 191 then you cannot add secondary CIDR from RFC 1918 IP ranges (10.0.0.0/8, 172.16.0.0/12, 192.168.1.1/16)
  - What that means is if your VPC address range of say 10.0.0.0/16,  (/16 is the shortest prefix) that you can have for your VPC. If you have this range allocated to your VPC, then you cannot habve the secondary CIDR range from other RFC ranges that is from 172.16 or 192.168 range
  - However, you could have the secondary CIDR range from 10.2.0.0/16 or 10.3.0.0/16 
- CIDR block must not be the same or larger than the CIDR range of routes any of the VPC route tables.
  - For example, if VPC primary CIDR is 10.0.0.0/16 and you want to associate a secondary CIDR in the 10.2.0.0/16 range. You already have a route with a destination of 10.2.0.0/24 to a virtual private gateway, therefore you cannot associate a CIDR block of the same range or larger. However, you can associate a CIDR block of 10.2.0.0/25 or /26 or smaller. 
- You can have a total of 5 IPv4 and 1 IPv6 CIDR blocks for VPC
- This example can be used when talking about pods running out of IPs and you want to extend your VPC address space.

- There's a table of allowed CIDR ranges. 
