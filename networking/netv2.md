# More networking basics

### IPSec VPN

- IPSec is a group of protocols
- it sets up secure tunnels across insecure networks
- … between two peers (local and remote)
- IPSec provides authentication so that only peers which are known to each other and can auth with each other can connect
- …and encryption

IPSec has 2 phases:

- IKE (Internet Key Exchange) Phase 1 (slow and heavy)
    - how keys are exchanged within a VPN
    - authenticate - pre-shared key (pass)/ cert
    - using asymmetric encryption to agree on and create a shared symmetric key
    - IKE SA created (phase 1 tunnel)
- IKE Phase 2 (fast and agile)
    - uses the keys agreed in phase 1
    - agree on encryption method and keys used for bulk data transfer
    - create IPSEC SA… phase 2 tunnel (architecturally running over phase 1)

Policy-based VPNs

- rule sets match traffic (a pair of SAs) SA > Security Association
- different rules/sec settings

Route-based VPNs

- target matching (prefix) e.g. send traffic for 192.168.0.0/24 over this VPN
- matches a single pair of SAs

## Border Gateway Protocol aka BGP

- A routing protocol used to control how data flows from point A through points B and C and arrives at dest point D.
- A system made up of lots of self-managing networks known as Autonomous systems (AS). An AS could be a large network or a collection of routers
- Routers are controlled by one entity, a network in BGP.
- AS are black boxes which abstract away from the detail and only concern with network routing in and out of AS. Each AS is allocated a unique number by IANA which is known as ASN.
- Now ASN (Autonomous system numbers) are the way that GBP identifies different entities within the network. It’s the way that BGP can distinguish between your network or your ASN and my network. BGP is designed to be reliable and distributed. It operates over TCP using port 179 (reliable) it includes error correction and flow control to ensure that all parties can communicate reliably.
- It isn’t automatic however. You have to manually create a peering aka a BGP relationship between 2 different AS (peering is manually configured)
    - once peered, 2 AS can communicate what they know about network topology,
    - A given AS will learn about networks from any of the peering relationships that it has and anything it learns, it will communicate out to any of its other peers. Coz of the peering relationship structure, you rapidly build up a larger BGP network where each individual AS is exchaning network topology info and that’s how the internet functions from a routing perspective. All of the major core networks are busy exchanging routing  and topology info between each other.
- BPG is a path-vector protocol it exchanges the best path to a destination between peers.. the path is called the ASPATH.
    - BGP doesn’t take into account link speed or condition and only focuses on paths.
    - For e.g. can we get from A to D using A, B, C and or is there a direct link between A & D.
    - It’s BGP’s responsibility to build up this network topology map and allow exchange between different AS.
- There are terms like:
    - iBGP = Internal BGP - routing within an AS
    - eBGP = External BGP - routing between AS’s (this type often used with AWS)
- Example of BGP:
    - We have 3 cities in Australia with 3 different routes. Each one has a dfiferent ASN (one has ASN 200, ASN 201 and ASN 202). Each use a different router range (10.16.0.1/16, 10.17.0.1/16, 10.18.0.1/16)
    - So each AS can have peering relationships configured so ASN 200, 201 and 202. So assume we have linked all 3. Each peer will exchange the best path that they have to a destination with each other.
    - All 3 AS can talk to both the others and this has been configured automatically once the BGP peering relationships were set up between each of the AS. This is a ring network so there are 2 ways to get to every other network, clockwise and anti-clockwise.
    - AS usually advertise the shortest route that they’re aware of to any other AS that they have peering relationships with.
    - At the end, there are indirect routes at the bottom of each route table have a longer AS path. These are non-preferred because it’s not the shortest path to the destination.
    - By default, BGP always uses the shorter path as the preferred one.
    - There are cases where you want a router to use a different path. However, BGP doesn’t take into account performance or condition because it’s the shortest path will always be used but there is a technique if you want to do this
        - this technique is called AS path prepending: which means you can configure a certain link/path to look worse than it actually is and this can be done by adding additional AS numbers to the path. You make it appear to be longer than it physically is.
        - AS path prepending can be used to artificially make the satellite path look longer making the fibre path preferred.
        - Remember BGP decides everything based on path length and so by artificially lengthening the path between a certain route, it means that another path will learn a new route which will be the shortest path.
- In, summary as BGP autonomous system advertises the shortest path to a destination that it’s aware of to all other BGP routers that it’s paired with. It might be aware of more paths but it only advertises the shortest one.
- It means that all BGP networks together to create a dynamic and ever-changing topology of all interconnected networks. It’s how many large enterprise networks function. It’s how the internet works and it’s how routes are learned and communicated when using Direct Connect and dynamic VPNs with AWS.

**Another example of a BGP:**

### ISPs as Autonomous Systems:

- **ISP-A (AS1)**: A large internet service provider serving customers in North America. It has its own network infrastructure (routers, servers, cables, etc.) and an Autonomous System Number (ASN), say AS1.
- **ISP-B (AS2)**: Another major ISP, but it serves customers in Europe, with its own infrastructure and ASN, say AS2.

### Interconnection for Global Internet Access:

- **Situation**: A user with an internet subscription from ISP-A wants to access a website hosted in ISP-B's network.
- **Role of BGP**: Here's how BGP and ASes come into play:
    1. **Routing Information Exchange**: ISP-A and ISP-B use BGP to share information about what networks they can reach and the best routes to take.
    2. **Path Selection**: BGP algorithms in ISP-A's routers determine the best path to reach the website hosted in ISP-B's network. This decision is based on factors like the number of ASes the data must pass through (AS path), network policies, and traffic conditions.
    3. **Data Transmission**: Once the best path is chosen, data from the user's computer travels through ISP-A's network, reaches ISP-B's network via inter-AS connections (possibly through other intermediate ASes), and finally arrives at the server hosting the website.
    4. **Response Back**: The website server responds, and the data travels back through the selected route to the user's computer.

### Key Points in This Example:

- **Autonomy**: Each ISP, as an AS, independently manages its network and decides the best way to route traffic within and outside its network.
- **Global Connectivity**: Despite being separate entities, ISP-A and ISP-B can communicate and route traffic between each other efficiently, thanks to BGP.
- **Dynamic Routing**: BGP continuously updates the routing information. If a usual path becomes congested or unavailable, BGP can dynamically change the route to maintain connectivity.

## Fibre Optic Cables
