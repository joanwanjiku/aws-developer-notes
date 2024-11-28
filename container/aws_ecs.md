# ECS
- Highly scalable container management service that facilitates running, stopping and managing containers on a cluster.
- Launching docker containers on AWS = Lauch ECS Tasks on ECS clusters.
- Containers are defined in a task definition used to run individual tasks.
- Run tasks and services on a serverless infrastructure managed by fargate.

## EC2 Launch Type
- Must provision and maintain the infrastructure.(EC2 instance)
- Each EC2 instance must run an ECS agent to register in the ECS cluster
-  **IAM Role:- Uses EC2 instance profile**:-, used by ECS Agent, make API calls to ECS service, send container logs to cloudwatch logs, Pull Docker image from ECR.
- Accommodate ECS service scaling by adding EC2 instances
1. Auto scaling group scaling :- Scale your ASG based on CPU utilization,
        Add EC2 instances over time
2. ECS Cluster Capacity provider:- Used to automatically provision and scale the infrastructure for your ECS Tasks.
-  Binpack(min cost, hence place tasks based on the least available amount of cpu and memory ), spread( place task based on  the specified value), random
- placement strategies can be mixed together
- placement contraints:- distinctInstance(place each task on a different container instace), memberOf(place tasks on instances that satisfy an expression)


## EC2 Fargate
- Serverless, No provisioning of infra(EC2 instances).
- Just create task definitions.
- AWS just runs ECS tasks for you based on the CPU/RAM you need.
- To scale, just increase no of tasks.
- **IAM Role:- Uses EC2 task Role**:- Allow each task to each role, use different roles for the different ECS Services you run. Task Role is defined in task definition

### Load Balancer Integrations
- 

### Data Volume(EFS)
- Mount EFS file systems onto ECS tasks,
- works for both launch types.
- Use caes:- persistent multi-AZ shared storage for your containers
- NB:- S3 cannot be mounted on a file system

### ECS Service Auto Scaling
- Automatically incr/decre the desired number of ECS tasks.
- Uses AWS application autoscaling.

### ECS Rolling Updates
- When updating from v1 to v2, we can control how many tasks can be started and stopped and in which order.  

### Task Definitions
- Metadata in JSON to tell ECS how to run a Docker container.
- I contains:- Image Name, Port bidding, env varibales, IAM Role, Networking information, logging configuration
- we get a dynamic host port mapping if you define only the container port. The ALB finds the right port on your EC2(load balancing in EC2 lauch type)
- To enable random host port, set host port = 0 (or empty), which allows multiple containers of the same type to launch on the same EC2 container instance.
- Each task has a unique private IP, only define the container port(Load balancing in Fargate)
- ECS task role are defined per task definition.
- ENV Var:- can be hardcoded, Use SSM parameter Store or secrets manager for sensitive variables, and shared configs. S3 for bulk files
- Data Volumes:- Bind mounts.
- 
### AWS CoPilot
- CLI tool to build,release, and operate prod-ready containerized apps.
- Runs apps on AppRunner, ECS and fargate
- Provisions all required infrastructure
- Deploy to multiple envs

# EKS
- It is a way to launch managed kubertes clusters on AWS
- It's an alternative to ECS, supports EC2, if you want to deploy worker nodes or fargate to deploy serverless containers
- Use Case:- 
