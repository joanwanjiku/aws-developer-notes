# EventBridge
- You can schedule cron jobs.
- Event rules can react to a service doing something.
- Sources:- S3 event, CodeBuild, CloudTrail(any API call)
- You can filter events
- Destinations:- Lambda, step functions, Codepipeline, EC2 actions, SNS e.t.c
- AWS integrates with Saas partners to partner Event Bus, make custom app that sent=d events to Custom event bus
- Ability to replay archived events
- Can archive events
## Schema Registry
- EventBridge can analyze the events in your bus and infer the schema, 
- The registry, allows you to generate code for your application that will know in advance how data is structured in the event bus

### Resource-based policy
- Manage permissions for a specific event bus
- Use Case:- allow/deny events from another AWS account or AWS region
- Use Case:- aggregate all events from your AWS organization ina single AWS account or region.


