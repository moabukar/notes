# DNS & Route 53

A practical example of how you might use AWS Route 53 in a typical workflow. Imagine you have a website that you want to make accessible via a custom domain name, like www.myapp.com.

# AWS Route 53 Workflow Example

## Step 1: Domain Registration
- **Action**: Register a domain name.
- **Route 53 Process**: Use Route 53 to search for and register a domain name (e.g., `myapp.com`). AWS handles the registration and charges an annual fee.

## Step 2: Setting Up Hosting
- **Action**: Host your website.
- **Outside of Route 53**: Host your website on an AWS service like an EC2 instance or an S3 bucket.

## Step 3: Configuring Route 53
- **Action**: Set up DNS management.
- **Route 53 Process**: 
  - Create a hosted zone in Route 53 for your domain.
  - Add DNS records, such as `A` or `AAAA` records, to point your domain to your hosting service's IP address or endpoint.
  - Optionally, set up `MX` records for email services.

## Step 4: Nameserver Configuration
- **Action**: Update nameservers.
- **Route 53 Process**: 
  - Route 53 assigns nameservers for your domain.
  - Update the nameservers with your domain registrar if your domain was registered elsewhere.

## Step 5: Health Checks and Traffic Routing (Optional)
- **Action**: Configure health checks and traffic policies.
- **Route 53 Process**: 
  - Create health checks to monitor your application's availability.
  - Use DNS failover and traffic routing policies like geolocation for optimized user access.

## Step 6: Accessing Your Website
- **Action**: Users visit your website.
- **How It Works**: 
  - DNS queries for `www.myapp.com` reach Route 53 nameservers.
  - Nameservers respond with the IP address from your A record.
  - The user's browser loads your website from the appropriate server.

## Conclusion

This workflow outlines the basic steps for using AWS Route 53 to manage web application access, including domain registration, DNS setup, and traffic management.


## Route53 notes

### Records

#### Nameserver (NS)
  - .com zone which is managed by Verisign
  - this zone has multiple nameserver records
  - these nameserers are how the delegation is done for amazon.com
  - they point at servers that are managed by amazon
  - these servers host the zone for amazon.com
    - inside this zone, are DNS records such as www, 
  - the same is true on the other side, the root zone has NS that point at the server that hosts the zone for amazon.com
  - so NS records are how delegation works end-to-end in DNS.


#### A & AAAA

  - Given a DNS zone, google.com, these type of records map a hostname to an IP address.
  - A record maps a hostname to an IPv4 address >> A record maps www >> 172.213.25.36 (IPv4 address)
  - AAAA record maps a hostname to an IPv6 address >> AAAA record maps www >> 2001:0db8:85a3:0000:0000:8a2e:0370:7334 (IPv6 address)

#### CNAME

  - CNAMES are used to map a hostname to another hostname. Host to host records
  - For example in google,com, there is a CNAME record that maps www.google.com to google.com
  - Let's say we have an A record called server which maps to an IP address 172.217.25.36 (IPv4 address)
  - So maybe this server providers multiple functions like ftp, mail and www:
    - Creating three CNAMES and pointing them all to the A server record means they will all resolve to the same IP address.
  - CNAMEs are used to reduce admin overhead, if the IP address of the server changes, you only need to update the A record, not all the CNAMES.
  - CNAMEs can not point directly to an IP address, they must point to a hostname. 

#### MX

  - Let's say you want to send an emal to hello@google.com
  - You have the google.com zone. Inside this zone, you have an A record with the name mail which points to an IP address.
  - Inside this zone is a collection of MX records. In this example, two records: (1) MX 10 mail, (2) MX 20 mail.other.domain
    - MX records have 2 main parts: a priority and a value.
    - The priority is a number, the lower the number, the higher the priority.
    - The value can be just a host like (1) so here mail means mail.google.com
    - So when you send an email on hello@google.com, it does an MX query on the google.com zone, then queries the records inside the zone. 
    - So here, mail is used first and then mail.other.domain is only used if mail is not functional. 
    - When it finds the MX record, the server gets the result of the query back and it uses this to connect to the mail server for google.com via SMTP and it uses this protocol to deliver the mail
  - So in summary, an MX record is how a server can find the mail server for a specific domain.
  - MX records are used constantly. Whenever you send an email to a domain, the server that is sending the email on your behalf is using DNS to do an MX lookup and locate the mail server to use

#### TXT
  - One common usage is to prove domain ownership
  - To verify that you own a domain, you can add a TXT record to the zone for that domain

#### TTL

- How long a DNS resolve should cache the record before requesting a fresh copy from the DNS server.

- Impact of TTL:
  - A shorter TTL means changes to your DNS records propagate more quickly across the internet, as DNS resolvers will check for updates more often. However, it also means more frequent DNS queries, which can slightly increase the load on DNS servers and potentially slow down the initial resolution time for users.
  - A longer TTL reduces the load on DNS servers and can speed up access for repeat visitors, as their DNS resolvers will cache the DNS record for longer. However, it also means that any changes you make to your DNS records will take longer to propagate worldwide.

- Updating Records: If you plan to change an important DNS record (like switching IP addresses for your website), you might lower the TTL beforehand to ensure the change propagates quickly.
- 
- Stability: For stable records that rarely change, a longer TTL can be set to reduce DNS queries and improve access speed for repeat visitors.

# TODO

- Common issues faced with DNS & Route 53
- Troubleshooting these issues in production and how to resolve them
