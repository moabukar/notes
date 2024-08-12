# Websockets


## Request Response

- Client > API GW > Lambda > DynamoDB
- Only client can invoke the server. Server cannot initiate connection to client.
- In the case of an e-commerce app or banking system - the frontend or the client invokes the API
- However, in the case of a chat app, a websocket connection is established between the client and the server. 
- Connection stays open and both client and server can send messages to each other.
- Can be acheived using API Gateway and Load Balancer
- Chat apps like WhatsApp, Facebook Messenger, Slack, Discord, Telegram, Chatbots, etc all use websockets
