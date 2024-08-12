# Messaging & Streamin

## Messaging

- Example of messaging could be using SQS
- System A > SQS > System B
- System A can be a financial system that sends a message to SQS
- System B can be a payment system that processes those records from the queue those messages into a database.
- Not as constant as streaming
- Can't really query the queue
- Messages are deleted once they are processed
- SNS & EventBridge are examples of messaging

## Streaming

- Website click stream > Kinesis Stream > System B
- System B can be a recommendation system that processes those records from the stream and sends recommendations back to the website.
- Streaming is a constant flow of data
- You should be a able to query and run analytics on the stream (you can use something like Kinesis Data Analytics), run SQL queries & build dashboards too
- Once messages are processed in a stream, messages still remain for a certain amount of time
- Amazon MSK (Managed Streaming for Kafka) or Kinesis Data Streams are examples of streaming
- 
