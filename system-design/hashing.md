# Hashing

- Hashing is the process of feeding a message of any length into a hash function and getting out a fixed-size string.
- Input (like "hello") > Hash Function > Output (fixed size string like a12338cx7)
- Same input will always produce the same output.
- Little change in input should produce a completely different output.
- Hash functions should be fast to compute.

## Table partitions

- DynamoDB uses consistent hashing to partition data across multiple servers.
- Hashing is applied on the primary key of the table to determine which partition the item will be stored in.
- So when you query an item, DynamoDB will use the same hashing function to determine which partition to look in.


## Challenges with hashing

### Example (using traditional hashing)

- You have 4 servers. You get request 1 with a hash of 1 = 10. `10 modulos 4 = 2. So you send request 1 to server 2`

- You have 4 servers. You get request 2 with a hash of 2 = 8. `8 modulos 4 = 0. So you send request 2 to server 0`

- If a server goes down or is added, you have to rehash everything and requests are remapped.
- You solve this problem with consistent hashing.

### Consistent hashing

- Consistent hashing is a special kind of hashing such that when a hash table is resized and consistent hashing is used, only `K/n` keys need to be remapped on average, where `K` is the number of keys, and `n` is the number of slots.

- A consistent hash ring is used. It is a circular array of slots. Each slot is assigned a node (server). Each node is assigned multiple slots.
- The requests are routed to the node/server that is clockwise to the hash of the request.
- When a node is added or removed, only the slots of that node are affected. The rest of the nodes are unaffected and are routed to the next clockwise node.
- In this method of hashing, only a few requests will get remapped when a node is added or removed. In the example of traditional hashing, all requests would have been remapped.
- The goal of consistent hashing is to:
  - uniformly distribute the load across all the nodes
  - minimize the number of keys that need to be remapped when a node is added or removed.

- Solving the issue of one server getting more load:
  - you solve this by doing virtual duplication of the nodes/servers on the hash ring. You are not adding actual servers but only a "virtual duplcation" so the load is distributed evenly.

- Sovling the issue of one server that has more CPU and more memory:
  - you run the hash function multiple times on the server and assign multiple slots to the server. This way the server will have more slots and will be able to handle more requests.
- Load balancers under the hood actually use consistent hashing.
- Also consistent hashing is used in backend servers where the record for a primary key should go to a specific server. For example, if you have a table with a primary key of user_id, you want all the records for a user to go to the same server. 
