# EC2
- scalable
- Storage
    1. Instance store volumes:- storage volumes for temporary data, which are deleted when you stop, hibernate, terminate your instance. Ephemeral
    2. EBS:- Elastic block storage, for persistent data. Can store Instances and EBS volumes in different regions and AZs.
- EC2 AMIs
    - provide information required to launch an instance.
    - launch multiple instances from a single AMI
    - Include:-
        1. One or more EBS snapshots, for instance-store-backed AMIs, a template for the root volume of the instance(OS, application server, applications etc).
        2. Launch permissions to control which AWS accounts can launch instances using the AMI
        3. Block device mapping that specifies volumes to attach to the instance when launched.
- EC2 instance Types
    e.g m5.xlarge:- m(instance class), 5(generation), xlarge(size within instance class)
    - General purpose:- balance btwn compute, memory, networking .. web servers, code repos
    - Memory optimized:- process large data sets in-mem. high performance relational/non-relational dbs,real time 
    - Storage optimized:- high frequency oltp systems, cache for in-mem db(redis), relational & no-sql dbs, data warehousing apps
    - Compute optimized:- high performance processors, batch processing, dedicated game servers, media transcoding, hpc
    Accelerated Computing
- EC2 user data
        #!/bin/bash
        yum update -y
        yum install -y httpd 
        systemctl start httpd
        systemctl enable httpd
        echo "<h1> hello world from $(hostname -f) </h1>" > /var/www/html/index.html
## EC2 purchasing 
- **On-demand**- pay as you go for compute capacity, also pay for other resources used e,g EBS volumes even on stoped state pay by second in running state.
- **Savings plan**:- reduce cost by making a 1/3yr commintment to a consistent amount of usage , USD/hr. Make commitment to compute usage instead of making commitments to specific instance configurations or compute services. 
    1. Compute savings plan:- reduces cost upto 66%, automaticaaly applied to EC2 instance regardless of family, region, AZ, size, tenancy
    2. EC2 instance savings plan:- reduce costs by upto 72% in exchange for commitment to usage of individual instance families in a region
- **EC2 Reserved Instances**:- not physical instances but billing discount applied to use on-demand instances in your account
    - Variables that determine reserved instance price
        1. Instance attributes:- instance type, region, tenancy, OS
        2. Term commitement:- 1yr/3yrs
        3. Payment Options:- all upfront, partial upfront, no upfront
        4. Offering class: Standard:- lock in to instance type, region, tenancy, OS. Convertible:- allows you to covert from one instance type to another.
- **Reserved Instances**:- reduce cost by making a 1 to 3 yr commitment to a consistent instance configuration, including instance type and region.
- **Spot instances**:- for background processing, tests jobs, EC2 rebalance recommendation:- Amazon sends a rebalance recommendation signal to notify that an instance is at risk of interruption.
- **EC2 scheduled instances**:- great for long term requirements which doesnot run constantly, specify a time window, instance ru during the time window, minimum commitment is 1 year.
- **EC2 capacity reservations**:- Allow you to reserve compute capacity for your EC2 instances in a specific AZ for any duration. Create capacity reservations at any time without entering into one year or three year term commitment, and capacity is available immediately. can cancel anytime. Charged on demand rate.workloads that run on specific AZ
    - Specify:-
        1. AZ in which to reserve capacity
        2. Number of instances to reserve
        3. Instance attributes:- instance type, tenancy, platform/OS
- **Dedicated instances**:- pay per hour for instances that run on single-tenant hardware.
- **Spot fleet**:- set of spot instance + (optional) on-demand instances.
    spot fleet will try to meet the target capacity with the price constraints 


## EC2 best practices
1. **Security**
    - Implement least permissive rules for your security group
    - Regularly patch, update and secure the OS and applications of your instance.
    - SG control incoming ang outgoing traffic in instances.
    - SGs only contain allow rules
2. **Storage**
    - EBS volumes for persistance data
    - Instance store for temporary data
3. **Backup and recovery**
    - Backup EBS volumes using EBS snapshots, create an AMI from your instance to save the configuration as a template for launching future instances.

### Default Monitoring Metrics
- CPU, DISK, Network, status check

## Public, Private, and Elastic address
- Private IPv4 addresses are not reachable over the internet
- Public IPv4 addresses allows communication between your instance and other AWS services that have public endpoints. When an instance is restarted, it is assigned another public IPV4 address
- Assign an elastic IP address to your public IP address to make the address persistent
- Can associate an IPv6 CIDR block with VPC and subnets and assign IPv6 addresses from the block to resources in your VPC.
- Elastic IP addresses are persistent and designed for dynamic cloud computing.Associated with AWS account in a specific region. 
- Elastic IPs charged when not associated.
- Cannot convert a public IPv4 address to an elastic IP address.
- Charged hourly for an elastic IP not associated with a running instance.
- Private IPv4 addresses can be used for commincation of resources within your VPC.


## ENI, ENA, EFA
- ENI:- Logical networking component in a vpc that represents a virtual network card. basic NIC. Can have
    - A primary private IPV4 address from the IPv4 address range of your VPC
    - One or more secondary private IPv4 addresses from the IPv4 address range of your VPC.
    - One elastic IP address per IPv4 address.
    - One or more IPv4 address
    - One or more IPv6 address
    - A mac address
    - One or more Sg
    - Bound to a specific AZ
    - attach/detach to ec2 instances to do failover.
    - 
    **Usage Scenario**
    1. Web servers, database Servers
    2. No high performance requirements
    3. supported on all instance types
- ENA:- uses a single root I/O virtualization(SR-IOV) to provide high-performance networking capabilities.
    - optimized to deliver high throughput per second
    - traffic can traverse subnets
    **Usage Scenario**
    1. Applications that require higher bandwidth and lower inter-instance latency
    2. Supported on limited EC2 types(HVM only)
    3. supported on all instance types
- EFA:- accelerate HPC and machine learning capabilities.
    - ENA with added capabilities.(additional OS-bypass functionality)
    - traffic is limited to a single subnet and is not routable
    **Usage Scenario**
    1. Web servers, database Servers
    3. tightly coupled applications

## AMI Images
- Information needed to launch an instance. Used when you need to launch multiple instances with same configuration
- AMI includes:
    1. One or more EBS snapshots or for instance-store-backed AMIs, a template for the root volume of the instance
    2. Launch permissions that include AWS accounts that can use the AMI to launch instances.AMI can be shared across accounts, but launch permissions are given to the accounts
    3. Block device mapping that specifies volumes to attach to the instance when launched.
- Can copy an AMI within same AWS region or to different AWS regions.


## Copying AMI
- Copy AMI within/across region and across accounts.
- Copy AMI with encyrpted snapshots and change encryption during copy.
- Copy of an AMI has a unique ID. 
- Launch permissions, user-defined tags, and S3 buckets are not copied from the source AMI to the copy.
- AWS marketplace AMIs cannot be copied across accounts

### Cross-region copying
- launch consistent instances in different regions from same AMI
- Launch instance from an AMI resides in the same region as the AMI.
- Changes made to the source AMI, must recopy source AMI to the targets.
- Update source AMI resources to support running in different region

### Cross-account copying
- Shared AMIs does not change ownership of AMI, owning account charged for the storage in the region.
- If you copy an AMI that was shared with your account, you become the owner of the target AMI in your account.

## EC2 Hibernate
- When an instance is hibernated, it saves ram contents to the EBS root volume.
- Must be a supported instance family, M,C,R,T
- Ram size must be less than 150GB
- Bare metal instances are not supported.
- Supported AMIs:- Amazon linux 2,Windows server AMIs, Ubuntu Ionic AMI.
- Root Volume must be EBS. EBS volume types, ggp SSD(ggp2 and ggp3)or provisioned iops(io1, io2), must be encrypted.
- Hibernation must be enabled at launch, cannot enable hibernation on running instance.
- Cannot change the instance size, type of instance with hibernation enabled.
- Cannot hibernate an instance in ASG, used by amazon ECS, or for more than 60days.
- AWS updates cannot be applied to instances in hibernation.
# EC2 Placement groups
- Determines how instances are placed on the hypervisor upon launch
    - **Cluster**:- instances launched in the same AZ, recommended launch same type of instances at same time. Launch instances that support enhanced networking.
                    if you try to launch more than one instance type, in the placement group you may get an insufficient capacity error.
    - **Spread**:- instances launched in different hypervisor in different AZ, reduces the risk or impact of underlying hardware failure, can launch diffrent
                    instance types at diffrent time. Maximum is 7 running instances per AZ per group. Not suppoerted for dedicated instances and hosts
    - **Partition**:- Like spread but designed when more than 7 instances are needed in an AZ.

## Elastic Network interface
- Component in a VPC that represents a virtual network card, you can create a network interface, attach/detach it to an instance or another instance. The attributes of a network interface follow it as it's attached or detached from an instance and reattached to another instance. When you move a network interface from one instance to another, network traffic is redirected to the new instance.

## Elastic Fabric Adapter
- An Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications. EFA enables you to achieve the application performance of an on-premises HPC cluster, with the scalability, flexibility, and elasticity provided by the AWS Cloud.

## Elastic Network Adapter
- Elastic network adapter supports network speeds upto 100Gbps for supported current instance types. The Elastic Network Adapter (ENA) driver publishes network performance metrics from the instances where they are enabled. You can use these metrics to troubleshoot instance performance issues, choose the right instance size for a workload, plan scaling activities proactively, and benchmark applications to determine whether they maximize the performance available on an instance.



1. Choose an AMI. It includes:-
    - A template for the root volume.
    - Launch permissions that control which AWS accounts can use the AMI to launch instances.
    - A block device mapping that specifies the volumes to attach to the instance when it is launched.
2. Choose instance type
3. Configure instance details
4. Storage
5. Tags 
6. Security group > launch
- The monitoring tab displays cloudwatch metrics for your instance.
- Actions > Monitor and troubleshooting > System log displays console output for your instance.

## Updating your security group
SG acts as a firewall to control traffic in and out your instance.

## Resize your instance type
Before resizing your instance type, you must stop your instance.

## EC2 limits

## EC2 Metadata
- IMDSv2 uses session-oriented requests. With session-oriented requests, you create a session token that defines the session duration, which can be a minimum of one second and a maximum of six hours. During the specified duration, you can use the same session token for subsequent requests. After the specified duration expires, you must create a new session token to use for future requests. ` TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
	&& curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/`


