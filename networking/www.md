### What happens when you type a URL like `google.com` in the browser and press enter?

## DNS Resolution

### Initial Cache Check
- **Local DNS Cache**: Check if "http://www.google.com" is in the Linux client's cache.
- **DNS Server Query**: If not in cache, query the DNS server in `/etc/resolv.conf`.

### DNS Resolver Process
- **Router as Resolver**: The local router queries Comcast’s DNS server.
- **Root Server Query**: Comcast forwards the request to the root servers for ".com".

### Domain Name Servers
- **Authoritative Servers**: Root servers refer to the authoritative name servers for "google.com".
- **IP Address Resolution**: These servers provide the IP address from their zone files.

### DNS Records and Caching
- **Record Types**: Includes A, MX, CNAME, NS, SOA, PTR, etc.
- **Server Caching**: Each server may cache responses for efficiency.

---

## Network Communication and Protocols

### TCP Handshake
- **Three-Way Handshake**: Establishes a TCP connection using SYN, SYN-ACK, ACK.
- **Reliability**: TCP ensures ordered and error-checked delivery.

### UDP Comparison
- **Usage**: UDP is used for speed-critical applications like streaming, where ordering is less critical.

### Routing and ARP
- **BGP Routing**: Internet routing uses BGP to find the optimal path.
- **ARP for Local Networks**: ARP resolves local IP addresses to MAC addresses.

---

## TLS/SSL (For HTTPS)

### TLS Handshake
- **Encryption Setup**: For HTTPS, a TLS handshake establishes a secure, encrypted connection.
- **Certificate and Key Exchange**: Involves exchanging encryption keys and verifying server certificates.

### SSL Certificates
- **Authentication**: SSL/TLS certificates authenticate the server’s identity.
- **CA Verification**: Certificates are checked against known Certificate Authorities.

---

## HTTP Process

### HTTP Request and Load Balancing
- **GET Request**: Browser sends an HTTP GET request to Google's server.
- **Load Balancer Function**: If Google uses a load balancer, it routes the request to a backend server.

### Web Server Processing
- **Apache Modes**: Apache may run in pre-fork or worker mode.
- **Response Preparation**: The server processes the request and sends back the HTTP response.

---

## Advanced Networking Concepts

### BIND Server for DNS
- **DNS Resolution**: BIND server software can be set up to host DNS records for domains.

### Apache SSL Configuration
- **Setting Up SSL**: Configuring SSL on Apache involves setting up virtual hosts and installing certificates.

### Network Security and Protocols
- **Security Measures**: Includes firewalls and intrusion detection systems.
- **Internal Routing Protocols**: OSPF and RIP within an AS; BGP for internet-wide routing.

### Encrypted vs. Unencrypted Traffic
- **HTTPS**: Encrypts data to protect during transit.
- **HTTP Vulnerability**: Susceptible to eavesdropping and attacks without encryption.

---

## Creating a private K8s cluster with AWS
