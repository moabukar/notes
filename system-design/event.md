# Synchronous vs Event-driven/async Architectures

## Synchronous architecture

- User > API Gateway > Lambda > DynamoDB

- Multiple users > API GW will scale > Lambda will scale > DynamoDB will scale

- So for example if the lambda fails, the user will get an error

- In a synchronous architecture, all components MUST scale together & can be expensive.
- Consumer needs to resend transaction for reproocessing

## Event-driven/async architecture

- User > API GW > SQS > Lambda > DynamoDB

- So for example if the lambda fails, the message will be sent to the SQS queue and the user will not get an error
- The message will be retried by SQS and then sent to the lambda again

- Each component can scale independently
- Retry built into SQS
- More cost effective than asynchronous architecture
- Since the architecture has a buffering system, the SQS, retries are built in and the consumer does not need to resend the transaction for reprocessing
- In case messages fail to deliver, you can set up a dead letter queue to store the messages that failed to deliver & reprocess them later

In real-world, use both synchronous & event-driven architectures together where applicable

Example ordering system:
- Order inserts can be done event-driven
- Order status retrieval can be done synchronously

