
## 1. What is SQS?

Amazon **Simple Queue Service (SQS)** is a fully managed messaging queue that enables decoupling between application components. It allows one part of an application (the sender) to send a message, and another part (the receiver) to process it later.

### Why Use SQS?
- **Decoupling Applications:** SQS separates the components of your application, allowing them to work independently.
- **Scalability:** Automatically handles large volumes of messages.
- **Reliability:** Messages are stored redundantly across multiple servers.

### Examples:
- **Order Processing:** A website places orders in an SQS queue, and the backend system processes them one by one.
- **Video Encoding:** Upload requests for video encoding are queued, and worker instances process them asynchronously.


## 2. Pull or Push?

SQS uses a **pull model**, meaning the consumer must poll the queue to receive messages. SQS does not push messages to the consumer.

### Why Pull Model?
- Gives the consumer control over when and how often messages are processed.
- Prevents overloading the consumer with too many messages at once.


## 3. Publisher and Consumer

### Publisher:
A **publisher** (or producer) is the component that sends messages to the SQS queue.

**Example:** A web application that sends order details (message) to an SQS queue.

### Consumer:
A **consumer** (or worker) is the component that retrieves and processes messages from the SQS queue.

**Example:** A background service that picks up order details from the queue and updates the inventory system.

### Workflow:
1. Publisher sends a message to the queue.
2. The message is stored in the queue until it is processed.
3. Consumer polls the queue, retrieves the message, and processes it.


## 4. Polling Duration

**Polling** is how a consumer retrieves messages from the queue.

### Types of Polling:
1. **Short Polling:** The consumer queries the queue for messages but may receive an empty response if no messages are immediately available.
2. **Long Polling:** The consumer waits (up to 20 seconds) for messages to arrive before returning a response. Long polling reduces empty responses and is more efficient.

### How to Choose:
- Use **short polling** for real-time systems where latency is critical.
- Use **long polling** to minimize costs and improve efficiency.


## 5. Visibility Timeout

The **visibility timeout** is the period during which a message is hidden from other consumers after being retrieved from the queue.

### Why is it Important?
It ensures that no other consumer processes the same message while the current consumer is working on it.

### Example:
1. A consumer retrieves a message and sets a visibility timeout of 30 seconds.
2. The message becomes invisible to other consumers for 30 seconds.
3. If the consumer does not delete the message within 30 seconds, it becomes visible again for processing.


## 6. Other Key Features of SQS

- **Maximum Message Size:** SQS messages can be up to 256 KB in size. Larger payloads can be stored in Amazon S3, with SQS storing a pointer to the data.
- **Message Retention:** Messages are retained for 1 minute to 14 days (default is 4 days).
- **Polling Frequency:** The consumer can poll the queue as frequently as needed, but over-polling can increase costs.


## 7. What is SNS?

Amazon **Simple Notification Service (SNS)** is a fully managed messaging service used for sending notifications. It enables publishers to send messages to multiple subscribers simultaneously.

### Why Use SNS?
- **Broadcast Notifications:** Send the same message to multiple systems or users.
- **Event-Driven Applications:** Trigger actions when certain events occur.
- **Real-Time Updates:** Notify users in real-time via SMS, email, or other channels.

### Example:
A stock trading platform uses SNS to notify users about price changes. The publisher sends a price update, and all subscribed users receive the notification.


## 8. Subscriptions in SNS

A **subscription** defines where and how a subscriber will receive messages from an SNS topic.

### How Subscriptions Work:
1. Create an SNS topic (a communication channel).
2. Add subscribers (endpoints) to the topic.
3. Publish messages to the topic.
4. SNS delivers the message to all subscribers.

### Supported Subscription Types:
SNS supports the following subscription endpoints:
- **HTTP/HTTPS:** Sends notifications as POST requests to an endpoint.
- **Email/Email-JSON:** Sends notifications as email or JSON-formatted email.
- **SMS:** Sends text messages to mobile phones.
- **Amazon SQS:** Delivers messages to an SQS queue for processing.
- **AWS Lambda:** Triggers a Lambda function with the message payload.

## 9. Subscription Limitations

- **Delivery Failures:** If a subscriber endpoint is unavailable, SNS retries the delivery for a limited time.
- **Filtering:** SNS does not have advanced filtering by default (use message attributes to filter).
- **Message Size:** The maximum size for an SNS message is 256 KB.
- **Batching:** SNS does not support message batching; each message is sent individually.

## 10. Differences Between SQS and SNS

| Feature                   | SQS                                | SNS                                |
|---------------------------|-------------------------------------|------------------------------------|
| **Model**                 | Queue-based (pull model).          | Publish-subscribe (push model).   |
| **Message Delivery**      | One consumer processes each message. | Messages are delivered to multiple subscribers. |
| **Use Case**              | Decoupling components.             | Sending notifications.            |
| **Message Persistence**   | Stored until processed or expired. | Delivered immediately to subscribers. |
