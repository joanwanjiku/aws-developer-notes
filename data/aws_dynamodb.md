# DynamoDB
- Fully managed, multi-region, multi-master NoSQL DBs

## Features
**Partition Key**
- Simple primary key composed of one attribute
- In a table with only the partition key, no two items can have same partition key value.
- Output(Internal hash function of partition key value) determine the partition

**Partition key + Sort key**
- Composite key.
- The output from the internal hash function of partition key value determines the partition(physical storage internal to DynamoDB) in which the item will be stored.
- All items with the same partition key are stored together, must have in sorted order by sort key value
- items can have same partition key, but should have different sort key.
- create table, add data, add items, edit items, delete items

**secondary index**
- lets you query the data in the table using an alternate key, in addition to the primary key
- optional. data can be read from the db through scan or query
- define only 5GSI and 5 LSI per table
![Global Secondary index VS local secondary index](images/GSILSI.png)

**Global secondary index**
- An index with a partition key and sort key that can be different from those on the table.
- *Global* means that the queries on the index can span all the data in the base table, across all partions

**Local secondary index**
- An index that has same partition key as the table, but a diiferent sort key.
- Local means that every partition of a LSI is scoped to a base table partition that has the same partition key value.
![Global Secondary index VS local secondary index](images/LSI_vsGSI.png)

- DynamoDB supports eventual consistent reads and strong consistent reads
- *Eventual consistent reads*:- When data is read after an immediate write, it might not be up-to-date.The most 
- *Strong consistent reads*:- When data is read, DynamoDB returns a response with the most up-to-date, reflecting all prior successful writes.
Throughput capacity for read and writes;- when a user defines throughput capacity, DynamoDB reserves the necessary resources to meet the read and write capacity your application requires.

    - Read capacity unit:- 1 strong consistent  read per second or 2 eventual reads per second for an item upto 4kb in size.
    - Write capacity unit:- 1 write per second for an item upto 1kb in size. No strong or eventual consistent
![RCU vs WCU](images/throughput_capacity.png)


**Auto-Scaling**
Adjusts provisioned throughput capacity on your behalf. It enables a table or GSI to increase its provisioned read and write capacity to handle sudden increases in traffic without throttling. Have to create a service-lonked iam role to access autoscaling

**Encryption**
- Encryption at rest can be enabled when creating a new dynamoDB table, not on existing table, once enabled, cannot be disabled.

**Query vs Scan**
Query finds the value based on primary key
Scan always scans the whole table or secondary index

**DynamoDB Streams**
- Captures data modification events in dynamoDB in real time and same order of occurence. When stream is enabled on a table, DynamoDB captures information about every modification to data items in the table
- Contains name of the table, the event timestamp, other metadata.
- Guaranteed to be delivered only once. Stream record is removed from the stream after 24hrs.
- Can be used with AWS lambda to create a trigger, or kinesis client library.

**DAX**
- DAX is a cache for dynamodb, delivers fats response times for accessing eventually consistent data. For read-heavy workloads, it provides increased throughput and operational cost savings as opposed to provisioning more read capacity units.