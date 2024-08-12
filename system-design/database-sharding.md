# Database Sharding aka Horizontal Partitioning

- ELB > EC2 (auto scaling group) > DB (RDS) (m5.large)
- If traffic increases, LB & EC2 can scale up but DB cannot scale up and there will be some throttling & app disruption.
- So how do we scale the DB?
- One way is vertical scaling aka scale up. You can upgrade the DB instance to a bigger instance type. But this is expensive.
- Another way is horizontal scaling aka scale out. You can add more DB instances. But how do you do this?
- Database sharding aka horizontal partitioning is a way to scale out the DB.
- You can shard the DB by user_id. For example, if you have 1000 users, you can shard the DB into 10 shards. Each shard will have 100 users. So you will have 10 DB instances.

- Now how do you know which shard to go to when you get a request?
- You can use consistent hashing to determine which shard to go to.

## Advantages of sharding

- Scaling horizontally supports distributed computing.
- Faster query response times.
- Limited blast radius during outage. If one shard goes down, only a few users will be affected.

## Disadvantages of sharding

- Unbalanced shards. Some shards will have more users than others.
- Resharding is difficult. If you want to add more shards, you have to move data around.
- Implementing sharding logic is an overhead.

Generally utilie a couple of other techniques before sharding:

- Caching
- Create more read replicas
