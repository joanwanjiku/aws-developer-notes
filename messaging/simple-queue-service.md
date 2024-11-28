# SQS
- Fully managed service, used to decouple apps.
- Attr:- Unlimited throughput, unlimited no. of messages in queue
- Default retention: 4 days, max:- 14days
- Limited of 256Kb per message
- Low latency
- Can have duplicate messages(at least once devlivery, occasionally)
- Can have out of order messages
- Use case:- decouple apps 

## SQS with ASG
- when the queue grows to a certain length(Metric), trigger a cloudwatch alarm that scales the ASG by x-amount.

### Secirity
- In-flight using HTTPS API, At-rest using KMS keys, client-Side
- IAM policies to regulate access to the SQS API 
- SQS access policies 

### Access Policy
- Cross-account policy, 
- Publish s3 Event notifications to SQS Queue.

### Message Visisbility Timeout
- After a message is polled by a consumer, it becomes invisible to other consumers
- By default it is 30 secs, after visibilty timeout is over, if message is not deleted, it can be polled.
- You can call ChangeMessageVisibility API to get more time to process a message, if the message requires more than 30secs to process.

### Dead Letter Queue
- If a consumer fails to process within VT, the message goes to a queue
- We can set a threshold of how many times a message can go back to a queue.
- After the maximum receives threshold is reached, the message goes into DLQ
- DLQ of a FIFO queue must be FIFO, and of a standard queue must be standard
- The messages expire after 14 days
- **Redrive to source**:- Feature to help consume messages of the DLQ, inspect them and queue back to the queue

### Delay queues
- Delay messages so consumers can't consume them upto 15mins

### Long Polling
- When a consumer requests messages from the queue it can "wait" for messages to arrive
- LongPolling decreses the number of API calls made to SQS while increasing the efficiency and decreaing the latency of your app
- Wait is btwn 1-20 sec
- can be set at queue or app level

### Extended client
- Use SQS extended client to send more than 256kb of messages,
- processing video files, upload on s3 then send message to SQS with link to process large message

### FIFO Queue
- maintain order of message
- Limited throughpu:300msgs without batching, 3000 msg/s with  
- queue has to end with .fifo
- Deduplication:- send message within 5 mins, the message isn't accepted in the queue.
    - Contet-based deduplication: will do a SHA-256 hash of the message body.
    - Explicitly provide a message deduplication ID
- Message grouping:- if you Specify same MessageGroupID, you can only have one consumer, message that share a common messsage group ID will be in the order of the group.


