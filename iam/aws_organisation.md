# AWS RAM
- helps you securely share the AWS resources that you create in one AWS account with other AWS accounts. If your account is within an organization, you can only share resources with all of the other accounts in the organization or only those contained by one or more specified organizational units.
## Benefits
- **Provides visibility and auditability**:- View the usage details for your shared resources through the intergration of AWS RAM with amazon cloudwatch and AWS cloudtrail.
- **Provides security and consistency**:- Simplify security management for your shared resources by using a single set of policies and permissions. If you were to instead create duplicate resources in all of your separate accounts, you would have the task of implementing identical policies and permissions, and then have to keep them identical across all of those accounts.

# AWS SQS
- Offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags.
## Queue types
1. Standard Queues.
- Unlimited Throughput – Standard queues support a nearly unlimited number of API calls per second, per API action (SendMessage, ReceiveMessage, or DeleteMessage)
- At-Least-Once Delivery – A message is delivered at least once, but occasionally more than one copy of a message is delivered.
- Best-Effort Ordering – Occasionally, messages are delivered in an order different from which they were sent.
2. FIFO Queues
- High Throughput – If you use batching, FIFO queues support up to 3,000 messages per second, per API method (SendMessageBatch, ReceiveMessage, or DeleteMessageBatch). The 3000 messages per second represent 300 API calls, each with a batch of 10 messages. To request a quota increase, submit a support request. Without batching, FIFO queues support up to 300 API calls per second, per API method (SendMessage, ReceiveMessage, or DeleteMessage).
- Exactly-Once Processing – A message is delivered once and remains available until a consumer processes and deletes it. Duplicates aren't introduced into the queue.
- First-In-First-Out Delivery – The order in which messages are sent and received is strictly preserved.
3. Dead-letter queues
- The main task of a dead-letter queue is to handle the lifecycle of unconsumed messages. A dead-letter queue lets you set aside and isolate messages that can't be processed correctly to determine why their processing didn't succeed. 

4. Visibility timeout
- When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message. Because Amazon SQS is a distributed system, there's no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application). Thus, the consumer must delete the message from the queue after receiving and processing it. Immediately after a message is received, it remains in the queue. To prevent other consumers from processing the message again, Amazon SQS sets a visibility timeout, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message. The default visibility timeout for a message is 30 seconds.

5. Delay Queues
- Delay queues let you postpone the delivery of new messages to consumers for a number of seconds, for example, when your consumer application needs additional time to process messages. If you create a delay queue, any messages that you send to the queue remain invisible to consumers for the duration of the delay period. The default (minimum) delay for a queue is 0 seconds. The maximum is 15 minutes.