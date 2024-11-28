# Elastic Map reduce
- managed cluster platform that simplifies running big data frameworks.
- process data for analytics purposes, and BI workloads.
- Transform and move large amounts of data into and out of the other AWS datastores e.g redshift and DBs
- EMR cluster are launched by either cloudformation or lambda, 
- Pay for the cluster instances only for the time they are running.
- EMR cluster will have 
    1. 1 master node:- EC2 instance, manages the entire cluster, runninng s/w components to coordinate distribution of data and tasks across other nodes
        a. core nodes:- used for tracking tasks, and store data in the HDFS
        b. task nodes:- optional, have no HDFS involvement, they do not run task trackers(core) just run the tasks. Spot instances are used for task nodes
    2. 
- EMR Storage:-
    1. Hadoop distributed file system:- distributes data it stores across instances in the cluster
    2. EMR File system;- directly access data stored in s3 as if it were a file system like HDFS, can persist beyond the lifetime of the cluster, lower performance than HDFS.
    3. Local file system
- EMR cluster management
    YARN:- centrally manage cluster resources. Agent on each node that keeps the cluster healthy and communicates with EMR.
- Data processing frameworks
    - Hadoop mapreduce
    - Spark:- cluster framework
## MapReduce
big data frameworks uses thic technique to process large data sets

# AWS Athena
- A serverless interactive querying service makes it easy to analyze data in S3 with standard SQL
- Source data is not transformed. Ideal for when data need not be transformed.
- Ideal for querying VPC Flow logs, cloudtrail and ELB logs
- Integrates with AWS glue to create tables and schemas from different data sources. Glue performs(ETL) jobs on the data by loading them through Athena.
- Athena querys the recent version. If the data is not encrypted, it can be stored in a different region from the primary region where you run athena. If 
encrypted, it must be stored in the same region, user/principal who creates the table in athena must have the appropriate permissions to decyrpt the data.
Does not support diffrent storage classes.

# Redshift
- Fully-managed petabyte scale data warehouse.
- Collection of computing resources called nodes, which are organized into a group called cluster, Each cluster runs an amazon redshift engine and contains one or more dbs.
- It is an OLAP(designed for complex queries to analyze aggregated historical data), not OLTP.
- Provides SQL-like interface with JDBC/ODBC connections
- It is a provisioned product, it is not serverless.
- It uses cluster architecture. It runs in one AZ hence not HA by design.
## Redshift Components
1. Cluster:- a set of nodes, which consists of a leader node and one or more compute nodes.
    1. **Leader node**:- receives, parses queries from client applications and develops query execution plans.Coordinates the execution of the plans with compute
        nodes, aggregates the results and handles external communication with client applications
    2. **Compute nodes**:- execute the query execution plans and transmit data among themselves to serve these queries. Have dedicated RAM, CPU and disk space.
        - *Dense Storage node type*:- large data workloads and use HDD storage
        - *Dense Compute node types*:- Optmisized for performance-intensive workloads.
## Redshift Workload management
- Enables users to manage priorities within their workloads so that short, fast-running queries are not stuck in queues behind long-running queues.
- WLM defines multiple query queues and route the queries to the appropriate queues at runtime.
- Redshift configures one queue with concurrency of five and one superuser queue with concurrency of one. You can configure upto 8 query queues.

# Amazon Kinesis
- Fully managed set of AWS services that allows you to stream data to your apps.
- Cost-effective process your streaming data at any scale. Process/analyze data as it arrives and use the data instantly in machine learning, analytics.
    1. Kinesis Data streams:- Collects and processes data streams in real-time. Makes use of shards, each shard has its own perfomance with 1MB/s ingestion capacity,
        2MB/s consumption capacity. Can increase the Num of shards to improve performance, Pricing is based on the num of shards. **Use Scenarios** include app logs, 
        market data feeds, web clickstream data. They describe response time for data intake and processing as real-time, hence processing must be lightweight. Create
        a stream, you can specify the number of shards
        - Producer puts data into the stream, put data into stream, specify the stream, partition key and data blob. Part key dtemines the shard that blob will be put into. shards hold part key,
        - Consumer consumes data out of data stream. 
        ![Kinesis Data stream](images/kinesis_data_streams.png)

    2. Kinesis Data firehorse:- Fully managed service, delivers near real-time streaming data to destinations such as redshift, S3, Splunk, Elasticsearch, kinesis data analytics, HTTP endpoints. Cannot write directly from kinesis data streams to this dest, use firehorse. Can configure it to transform your data before delivering to your dest. Serverless. Pay as you go service, pay per volume of data. Firehose buffers the data for 1MB of data in 60 seconds before delivering it to destination
        - Firehose to S3:- delivers to S3 directly, can transform data before deliverying to S3 bkt, can store original in another S3 bkt, 
        - Firehose to Redshift:- delivers to S3 first, then runs a redshift COPY command.
        - Firehose to elasticsearch:- Delivers directly to an elasticsearch cluster, can optionally backup to S3 concurrently
    3. Kinesis Analytics:- Can stream data from firehose and data streams only. Output is data streams can use lambda and firehose. Can use standard SQL-queries on the streaming data in near real-time to build applications that transform and provide insight into your data. **Use Cases** real-time SQL processing, time-series analytics:- election data, e-sports, real-time metrics
    4. Kinesis video streams:- Fully-managed service that streams live video from devices to the AWS cloud where you can build applications for real-time video processing or batch-oriented video analytics.