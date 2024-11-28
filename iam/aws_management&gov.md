# Cloudwatch
- It is a monitoring and management service. Provides you with data and actionable insights to monitor your applications, understand and respond to system-wide performance changes, optimize resource utilization and get a unified view of operational health.
1. Default metrics:- for EC2 metrics(CPU, disk, Network, Status)
2. Custom metrics:- for EC2 metrics(memory, disk, swap)

## Cloudwatch alarms
- A metric alarm watches a single cloudwatch metric or the result of a math expression based on cloudwatch metrics. The alarm performs one or more actions based on the value of the metric or expression relative to a threshold over a number of time periods. the action can send a notification to an SNS topic, perform an EC2 action, An autoscaling action etc
- A composite alarm takes into account the alarm states of other alarms that you have created. The alarm goes into ALARM state only if all conditions of the rule are met. Using composite alarms can reduce alarm noise. You can create multiple metric alarms, and also create a composite alarm and set up alerts only for the composite alarm. For example, a composite might go into ALARM state only when all of the underlying metric alarms are in ALARM state.
### Alarms states
1. OK:- the metric or alarm is within the defined threshold
2. ALARM:- The metric or expression is outside of the defined threshold
3. INSUFFICIENT_DATA:- The alarm just started, the metric is not available, or not enough data is available for the metric to determine the alarm state
## Cloudwatch events
- Realtime 
- An event indicates a change in your AWS environment.
- Target processes events, can include EC2 instances, AWS lambda functions.
- Rules matches incoming events and routes them to targets for processing.
## Analyze
- Process cloudwatch log events through kinesis and lambdas for custom processing and analysis.
- metric math enables you to query multiple cloudwatch matrics and use math expressions to create new time series based on these metrics.

