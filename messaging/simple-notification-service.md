# SNS
- One message, multiple receivers.
- Pub/Sub patterns
- Event producer only send message to one SNS topic
- Can have upt 12,500000 event subs
- Send to Emails, SQS, Lambda, Kinesis Data fireHorse

## SNS + SQS Fan Out
- Push once in SNS topic, receive all in SQS queues, fully decoupled, allows for data persistence, delayed processing e.tc
- SQS queue policy should allow for SNS to write.
- Cross-Region Delivery: works with SQS in other regions.
- **S3 to multiple queues**:- for the same combination of event type, and prefix you can only have one S3 event rule, to send same S3 event to multiple SQS queues, use fan out
- **SNS to S3 through Kinesis Data Firehorse**
- Can do message filtering:- JSON policy used to filter messages sent to SNS topic's subscriptions
