check : https://www.ibm.com/topics/message-brokers

Concept of Message Broker:

Broker means a third party/middleMan that helps to perform or achieve our goal

e.g.
Case 1. An Owner is searching for a tenant to rent out his apartment. So, to make his life easy he hired a broker who will help him in getting a tenant.

Case 2. Service A (Provider)wants to communicate with service B(Consumer). But Service A doesn’t know anything about Service B
so hire a message broker, which helps service A and service B to communicate each other

What are Message Brokers, and why do we need these?
 
A message broker is a software component that acts as an intermediary between different applications / systems / services, This allows them to communicate with each other and exchange information. even if they’re written in different programming languages

why need ?
For two applications to talk with each other, we first need some interface. Without it, it won’t be possible for them to communicate. That is why we need to define the interface. The defining thereby includes the transport way and the transport protocol selection like HTTP, MQTT, or SMTP.
In addition, it is necessary to agree on the message structure. As long as both systems agree on a standard message structure, they can exchange data independently of the respective implementation within the systems. The implementation of the systems can change over time as long as the interface retains the same structure.
A message broker is an intermediary where all messages send through. Thereby we achieve an additional decoupling between transmitter and receiver. A sender sends a message to the message broker, and the message broker will pass it on to the recipient. A key advantage here is that the message broker does not need to know where the consumer is in the network.
Moreover, the message broker acts as a buffer — it will hold on to the messages until the consumer is ready to work.

Why use a message broker?
 
Decouple Systems: It helps decouple the sender and receiver. Rather than sending messages directly to another application, a sender publishes messages to a message broker without knowing who the receiver is. Similarly, a receiver can subscribe to messages from the broker without knowing the sender. This reduces dependencies between applications and makes it easier to change or scale them independently.

Reliability: A message broker can help ensure reliable message delivery. It can use features such as message queues, message persistence, and acknowledgment mechanisms to ensure that messages are delivered and processed in a reliable manner.

Support multiple patterns and protocols out of the box: Message broker typically implements different types of messaging patterns, such as point-to-point, publish/subscribe, or request/response. A message broker can help convert messages from one protocol or format to another. This can be useful when communicating between applications that use different communication protocols or data formats.


Examples of message brokers
The most popular message brokers are RabbitMQ, Apache Kafka, Redis, Amazon SQS, and Amazon SNS.


What are the disadvantages of using the message broker?
 
*Increased system complexity*
Introducing a message broker in your system is a new element to your whole system architecture. Because of that, there are more things you have to take into account, such as maintaining the network between components or security issues.  Additionally, a new problem arises related to eventual consistency. Some components could not have up-to-date data until the messages are propagated and processed.
  
*Debugging can be harder.*
Let’s say you have async multiple stages of processing a single request using the message broker. You send something but did not receive a notification. Searching for a cause of failure can be a challenge as every service has its own logs. Keep in mind to add some message tracing facilities alongside implementing systems using message broker.

How do services in a microservice architecture communicate

Services use Asynchronous way for inter-communication by exchanging messages over messaging channels/message broker and to do this there are lots of Styles as follow:

Notifications — a sender sends a message to a recipient but does not expect any/nor even get any reply.
Request/response — a service sends a request message to a recipient and expects to receive a reply of the message promptly.
Request/asynchronous response — a service sends a request message to a recipient and expects to receive a reply of the message eventually.
Publish/subscribe — a service publishes a message to zero or more recipients
Publish/asynchronous response — a service publishes a request to one or recipients, some of whom send back a reply but some don’t.


