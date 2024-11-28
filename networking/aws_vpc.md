# VPC
- CIDR:- method for allocating IP addresses. 
- max 5 vpcs per region.
- max CIDR per VPC is 5, min size of CIDR is /28, max size is /16
- AWS reserves first 4 and last 1 ip addresses

## Internet Gateway
- Scales horizontaly
- 1 vpc only associated with 1 IG
- The route table in the subnets must allow traffic from the IG for the subnet to ave access to the internet.

## Bastion Hosts
- A way to ssh into private subnets, bastion is in the public subnet,
- Bastion host SG, must allow inbound traffic from the internet on port 22 from restricted CIDR.
- SG of the instances, must allow the SG of the bastion host, or the private IP of the bastion host.

## Nat instance vs Nat Gateway
Nat instance
- customizable, User is in control of creating, managing multiple instances needed to be highly available. Ec2 instance with a natting capability, 
- must be launched in a public subnet.
- must disable EC2 setting:Source/destination check.
- must have elastic IP attached.
- They rewrite network packets, src IP is changed

## Nat Gateway
- Managed by AWS, high availability, no administartion
- pay per hour for usage and bandwidth.
- Can't be used by EC2 instance in same subnet(only from another subnet)
- Requires an IGW(Private subnet ->NatGW->IGW)
- 5 Gbps of bandwidth with automatic scaling to 34gbps\

## VPC Peering
- privately connect two vpcs using aws network. Make them behave as if same network
- must not have overlapping cidrs
- VPC peering is not transitive
- Can happen cross account and cross region.
- To send and receive traffic across a vpc peering connection, you must add a route to the peered VPC in one or more of your VPC route tables.

# AWS Privatelink
- Privatelink privides private connectivity between VPCs, AWS services and your on-premises networks, without exposing your traffic to the public internet
- Makes it easy to connect services across different accounts and VPCs to significally simplify your network architecture.

# VPC Endpoints
- The entry point in your VPC that enables you to connect privately to supported AWS services and VPC Endpoints services powered by Privatelink without requiring an IG, NAT gateway. Instances in your VPC do not require public IP addresses. 
    1. *Interface Endpoints*:- It is an Elastic network Interface with a private IP address from the IP address range of your subnet. Serves as an entry point for traffic destined to a service that is owned by AWS or owned by an AWS customer or partner. You are billed for hourly usage and data processing charges.
    2. *gateway endpoint*:- is a gateway that is a target for a route in your route table used for traffic destined to either Amazon S3 or DynamoDB from your VPC over the AWS network

# Site-to-Site VPN
- Allows communication between amazon VPC and on-premises network. Supports IPSec VPN connections.

# VPC Peering
- A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses. Instances in either VPC can communicate with each other as if they are within the same network. 
- You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account. 
- The VPCs can be in different regions (also known as an inter-region VPC peering connection).
- You cannot edit the VPC peering connection once it is created
- Cannot attach/detach VPC peering connection
#### How it works
1. a requestor vpc creates a VPC connection request.
2. The Acceptor VPC ensures the CIDR ranges are not overlapping and accepts the request.
3. The requestor adds a route to the acceptor VPC
4. The SG of the instances in the vpcs subnets have to be updated as well

## VPC Flow logs
- Feature that enable you to capture info about the IP traffic going to and out of network interfaces in your VPC.
- Can be published to cloudwatch or s3 bucket.
- Flow logs do not capture real-time log streams for your network interfaces.
- A new log stream(for cloudwatch logs) or log file object(for s3 bucket) is created for each new network interface as soon any traffic is recorded for that network interface.
**logs not captured**
- Traffic generated by instaces when they contact the amazon DNS server, if you use your own DNS server, then all traffic to that DNS server is logged.
- Traffic generated by windows instance for amazon windows license activation.
- Traffic to and from 129.254.169.254 for instance metadata
- Traffic to and from 129.254.169.123 for amazon timesync service.
- DHCP traffic
- Traffic to the reserved IP address for the default VPC router.
- Traffic between an endpoint network interface and a load balancer network interface.

#### Uses cases
Why traffic is not reaching an instance
Diagonise overly restrictive SG rueles

## NAT Instance vs NAT Gateway
When traffic goes to the internet, the source IPV4 address is replaces with the NAT address, When the traffic goes bac to the private instances, the NAT device translates the address back to those instances private IPV4 address, IPV6 traffic is not supported, use egress-only internet gateway