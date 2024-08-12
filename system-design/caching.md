# Caching

- User > API GW/LB > Backend > Database
- A cache implemented in front of the database can help reduce the load on the database. The backend can check the cache first and if the data is not found, it can query the database and store the data in the cache for future use.
- You only want to go to database when you absolutely have to because scaling up the database is relatively harder & expensive
- The cache can be implemented using Redis or Memcached
- Cache entries get deleted after a specified TTL (Time To Live) or when the cache is full and needs to make space for new entries
- Cache entries can be updated when the data in the database changes
- One misonception is restricted to backend but it can also be in different parts of the system like CDN, browser, etc

## Which caching service to use when?

- Use managed caching services like ElastiCache for Redis or Memcached

## Using caching on AWS services

- Client > API GW (Enable API cache) > Lambda > DynamoDB (Enable DynamoDB cache like DAX or DynamoDB Accelerator)
- Client > CloudFront (Enable CloudFront cache) > S3 (static content)

- Client > ELB > AKS (ElastiCache) > RDS


## Redis vs Memcached

- Both open sourced caching services
- Memcached is a much simpler caching service compared to Redis - Redis supports more complex use cases


## Caching Strategies

### Lazy Loading

- User > (Read) EC2 > (Cache hit) ElastiCache > (Cache miss then read from database) Aurora
- Only requested data is cached
- Data can become stale


## Write Through

- User > (Write) EC2 > (Write to cache) ElastiCache > (Write to database) Aurora
- Data can not become stale

Both can be used together in different parts of the system. For example, write through can be used for user data and lazy loading can be used for product catalog data.
