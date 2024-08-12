# SQL & NoSQL

## SQL aka RDBMS

- Tables have predefined schema
- Good for joins & complex queries
- Emphasises on ACID properties
- Generally scales vertically
- Examples Oracle, AWS RDS, MySQL, PostgreSQL
- Aurora can scale horizontally


## NoSQL

- Schemaless
- Generally not fit for complex multi-table queries
- Emphasis on CAP theorem
- Generally scales horizontally, AWS DynamoDB scales automatically
- AWS DynamoDB, MongoDB, Cassandra

Note: With the advent of technology, segregation of use cases is not as clear cut as it used to be. For example, MySQL can be used for key-value store, and MongoDB can be used for complex queries. And DynamoDB can also follow ACID properties.

You can run your favourite non-AWS DBs on EC2 but if you can, you should use AWS native Databases. 

## Amazon Aurora vs Amazon DynamoDB

### Amazon Aurora

- Aurora is a relational database, DynamoDB is a NoSQL database
- 5 times faster than standard MySQL databases, 3 times faster than standard PostgreSQL databases at 1/10th the cost
- Multi-master support for MySQL
- Serverless option via Serverless Aurora which scales automatically but not as scalable as DynamoDB

### Amazon DynamoDB

- Key-value document database
- Multi-master support
- In-memory caching via DAX
