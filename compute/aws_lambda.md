# Lambda
- serverless compute service, run apps without managing the underlying infrastructure.
- Executes code only when needed and scales automatically.
- Runs code on highly available compute infrastructure and performes all the administarion of compute resources, server and OS maintenance, capacity provisioning, automatic scaling, code monitoring and logging.

## components
*lambda function*:- function that processes events
*event sources*:- An event is a JSON-formatted document that contains data for a Lambda function to process.Any customer app that produces events e.g cloudwatch event, SNS topic, kinesis stream, S3
- Appropriate access policies must be assigned to the IAM role of the lambda function to be able to call the service
- Can be created within a VPC. Must make sure your VPC has sufficient elastic network capacity to support scale requirements. Use 
    *projected peak concurrent executions * memory in GB* to determine elastic network interfaces. With not enough ENIs, your lambda function will not scale as request increase and invocation errors will be seed.
*Deployment package*:- You deploy your lambda function using a deployment package, Two types of deployment packages.
    1. A .zip file archive that contains your function code and its dependencies. Lambda provides the operating system and runtime for your function.
    2. A container image that is compatible with the Open Container Initiative (OCI) specification. You add your function code and dependencies to the image. You must also include the operating system and a Lambda runtime.

### Lambda invovations
**Synchronous invocation**:- When you invoke a function synchronously, Lambda runs the function and waits for a response. When the function completes, Lambda returns the response from the function's code with additional data, such as the version of the function that was invoked. API gateway will also invoke lambda synchronously for many serverless applications. E.g Cognito, Aws step functions, Application ELB, Cloudfront
**Asynchronous invocation**:- Used by S3, SNS to process events. supports SQS, SNS, another lambda, eventbridge as destinations for asynchronous invocation.
As an alternative to an on-failure destination, you can configure your function with a dead-letter queue to save discarded events for further processing. A dead-letter queue acts the same as an on-failure destination in that it is used when an event fails all processing attempts or expires without being processed
**Event source mappings**:- To process items from a stream or queue, you can create an event source mapping.
- An event source mapping is a resource in Lambda that reads items from an Amazon Simple Queue Service (Amazon SQS) queue, an Amazon Kinesis stream, or an Amazon DynamoDB stream, and sends the items to your function in batches.
- Each event that your function processes can contain hundreds or thousands of items.you can configure Lambda to send a record of batches that failed processing to a queue or topic. Supports SQS standard queues and FIFO queues.
- By default, Lambda polls up to 10 messages in your queue at once and sends that batch to your function. 

### Lambda networking
- By default, Lambda runs your functions in a secure VPC. Lambda owns this VPC, which isn't connected to your account's default VPC. When you connect a function to a VPC in your account, the function can't access the internet unless your VPC provides access. 
- In this mode, lambda has access to public AWS services
- Lambda provides managed resources named Hyperplane ENIs, which your Lambda function uses to connect from the Lambda VPC to an ENI (Elastic network interface) in your account VPC.
- To give your function access to the internet, route outbound traffic to a NAT gateway in a public subnet. The NAT gateway has a public IP address and can connect to the internet through the VPC's internet gateway.

### Lambda layers
- Convenient way to package libraries and other dependencies that you use with your lambda functions. Reduces the sizes of uploaded deployment archives, making it easier and faster to deploy.
- A layer can contain libraries, a custom runtime, data, or configuration files. Layers promote code sharing and separation of responsibilities so that you can iterate faster on writing business logic.
### Lambda environment variables.
- By default, they are asscociated with $LATEST.
- Encyrpted by KMS

### Lambda versions
- A version includes;-
    - The function code and all associated dependencies.
    - The Lambda runtime that invokes the function.
    - All the function settings, including the environment variables.
    - A unique Amazon Resource Name (ARN) to identify the specific version of the function.
- You can use versions to manage the deployment of your functions.
- You can change the function code and settings only on the unpublished version of a function. When you publish a version, Lambda locks the code and most of the settings to maintain a consistent experience for users of that version.
- You can use a qualified(With version suffix) or an unqualified(without version suffix ($LATEST)) ARN in all relevant API operations. However, you can't use an unqualified ARN to create an alias.

### Lambda alies
- You can create one or more aliases for your Lambda function. A Lambda alias is like a pointer to a specific function version. Users can access the function version using the alias Amazon Resource Name (ARN).
- Has an unique ARN, can only point to a version not another alies.
- Can point an alies to a maximum of two versions. iff:-
    - Both versions must have the same execution role.
    - Both versions must have the same dead-letter queue configuration, or no dead-letter queue configuration.
    - Both versions must be published. The alias cannot point to $LATEST.
- Easier promotion of new versions of lambda functions and rollbacks when needed.
### Lambda logging and monitoring
- When using an AWS Cloudwatch rule to trigger a Lambda event, one of the multiple options you have to pass data onto your Lamba function is “Constant (JSON Text)”
- Lambda monitors functions on your behalf and sends metrics to Amazon CloudWatch. The Lambda console creates monitoring graphs for these metrics i.e invocation, performance, concurrency.
- You can use AWS X-Ray to visualize the components of your application, identify performance bottlenecks, and troubleshoot requests that resulted in an error. Your Lambda functions send trace data to X-Ray, and X-Ray processes the data to generate a service map and searchable trace summaries.

### Scenarios
- Real time file processing
- ETL jobs
- Serveless web application

# Lambda@Edge
- extention of lambda, compute service that executes functions that customize the content that cloudfront delivers, can have compute in the cloudfront cache. 
- Create functions in one region that executed globally without provisioning servers.
- Scales automatically more functions kick off to handle your request(Horizontally)
- supports nodejs and python.

## Use cases
- Inspect cookies to rewrite URLs to deliver different versions of a site for A/B testing.
- Leverage the User-agent header to return different objects to viewers based on requesting device, such as image size based on device capabilities.
- Inspect headers,token authorization, to external resources to confirm credentials
- Issue calls to fetch additional content to customize response.
- 
