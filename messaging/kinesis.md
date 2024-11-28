# Kinesis
- Makes it easy to collect, process and analyze streaming data in real-time.
- Use Case:- app logs, real-time big data analysis, ETL

## Kinesis Data Streans
- Strean big data in sysystem. made up of shards, should be provisioned ahead oh=f time
- Producers send data into data stream e.g apps, SDK, produces a Record(Contains a partition key and data blob), the partition key determines which shard the recors=ds goes into
- Consumers receives a record(partition key,sequence no, data blob)
- Retention btwn 1-365 days
- data inserted cannot be deleted,
- data sharing the same partition keys goes to same shard 
### Capacity Mode
1. **Provisioned mode**:- you choose number of shards provisioned, scale manually via API
    - you pay per shard provisioned per hour.
    - @ shard gets 1mb/s in and 2mb/s out.

2. **On-demand**:- No provisioning capacity
    - Scales automatically,
    - pay per stream per hour & data in/out per GB

### Security
- Control access via IAM policies
- VPC Endpoints available for kinesis to access within VPC
- Monitor API calls using cloudtrail


### Kinesis Producers
- Puts data into data streams. Records consist of Sequence number, part key, datablob (upt 1mb)
- Can be: AWS SDK, Kinesis Producer Library(C++, java, batch), Kinesis Agent
- Use a distributed partition key.
- *ProvisionedThroughtputExceeded*:- when exceeding amount of throughput per shard.
    - Solution:- Retries with exponential backoff, increase shards, highly distributed partition key.

### Kinesis Consumers
- Get Records from streams and process
- Can be:- AWS Lambda, Kinesis Data Analytics, Kinesis Data Firehouse, Kinesis Client Library, custom consumers(SDKs).
- Shared(Pull) 2Mbs per shard, shared by all consumers Vs Enhanced(Push) 2Mbs per shard per consumer mode 
- Lambda receives records in batches from data streams, process and save to dynamoDb.

### KInesis Client Library
- Java lib that helps read record from a kinesis data stream, each shard is read by one KCL instance
- **Shard splitting** :- used to incr the stream capacity, used to divide a hot shard.
- **Merge Shards**:- save costs by decreasing capacity.

## Kinesis Data Firehouse
- Receives from Kinesis data streams, SDK, Client, 
- AWS Destinations:- Amazon S3, Amazon Redshift(through a copy command from S3), Amazon Opensearch
- Stream to 3rd party apps and custom HTTP
- data transformation can happen within Firehouse through a lambda func.
- Pay for data going through Firehouse
- Near real time:- buffer interval is 0-900secs
- Suports many data formats
- Can send failed data or backups to s3

#### Streams VS Firehose
1. Streams:- ingest at large, Write custome code for producers/consumers, managed scaling, support replay capability
2. Firehose:- Load streaming data into S3, Redshift, Opensearch, near real time, no data storage,  doesn't support replay capability

## Kinesis Data Analytics
- Sources:- Data streams, Data Firehose
- Destination:- Data Streams, Firehose
- Enrich data using Amazon S3.
- Fully managed.no servers.
- Automatic Scaling.
- Use Cases:- Real-time dashboards, Real-time metrics, 
- **Kinesis Data Analytics for Apache Flink**:- used to pocess and analyze straming data
    - provisioning compute resources, parallel computation, automatic scaling
    - Flink does not read from Firehose(Use kinesis Analytics for SQl)

#### Ordering data in Kinesis
- 