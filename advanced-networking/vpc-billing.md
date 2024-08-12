### VPC Peering

- Connect two VPC, privately using AWS’ network 
- Make them behave as if they were in the same network 
- Must not have overlapping CIDR 
- VPC Peering connection is not transitive (must be established for each VPC that need to communicate with one another) 
- You can do VPC peering with another AWS account 
- You must update route tables ineach VPC’s subnets to ensure instances can communicate

### VPC Peering Cost Management

- Same AZ for a VPC peer connection is free
- Cross AZ transfer has ingress and egress charges
- Cross region: when peering VPC globally, you pay for inter-region data transfer
  - data is charged for as it exits or egresses a region
  - the ingress part of the same data flow that's going into the other region is not charged for
  - for traffic flow in the other direction, the same rules apply. Egress data is charged for, and ingress data as it enters the region is not charged for
- Comparing the costs for cross AZ and cross region, they are roughly similar.

### Transit Gateway Billing

- TGW is a hub and spoke network gateway. Things are attached to the TGW (VPCs, VPNs, Direct Connects)
  - Each attachment has an hourly charge while active (billed to attachment owner)
  - Per GB data processing charge for data SENT "from" each attachment (billed to attachment owner). Any data received by an attachment is FREE. 
- Cross region peerin aka inter-region peering:
  - Hourly attachment charge for each peering attachment
  - Per GB for data sent to TGW
  - Data received from TGW to attachments is FREE
  - Data SENT over a TGW has a per GB charge billed to the owner of the sending TGW (there is no charge at the receiving TGW)

### Direct Connect DX Billing

Scenario 1: DX to VPC (without DX Gateway and VPCs are in same region):

- A DX port has an hourly fee - based on the port speed and location
- Data transfer OUT has a per GB charge based on the source location and the DX location
- Data trainsfer IN is FREE of charge. So, from customer side of the DX to AWS VPCs is free

Scenario 2: DX to VPC (with DX Gateway and VPCs are in different regions):

- A DX port has an hourly fee - based on the port speed and location
- DX GW is FREE
- Data transfer IN from customer DX side through to DX GW to AWS VPC is FREE
- For any outbound data from AWS VPC to DX GW to customer DX side for each region, you pay a fee based on the source region and the associated DX location. 
- you pay per GB fees for all data transferred out of all VPCs in all regions connected to the DX GW and any public VIFs. 
- No double billing but a combination of all of the source region egress costs to the DX location. 
