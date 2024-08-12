# EC2 Enhanced Networking - SR-IOV (Advanced)

- Something which improves overall network performance of EC2 instances and is powered by something called SR-IOV (Single Root I/O Virtualisation)
- Networking is traditionally virtualised which added lots overhead which reduces performance. Physical networking means that the OS has direct access to the network hardware, generally a NIC (Network Interface Card).
- VMs aka EC2 instances share a physical network interface card (NIC). So VMs, generally, each physical network interface card (NIC) is shared by multiple VMs. 
  - The hypervisor mediates access (slow) between those and this has been quite slow
- 
