# Security services 
## AWS WAF
Helps protect web apps against common web exploits, that affect application availabilty, compromise security and consume excessive resources.
use cases:
- Monitor HTTP and HTTPS requests that are forwarded to an amazon API gateway, API cloudfront or an application load balancer.

## AWS Shield
Protects against DDOS
1. Standard shield:- protects network and transport layer against attacks
2. Advanced Shield:- provides protects for web applications running on amazon EC2, ELB, Cloudfront and Route 53 resources.

## AWS Resource Access Manager
AWS Resource Access Manager (RAM) helps you securely share your resources across AWS accounts, within your organization or organizational units (OUs) in AWS Organizations, and with IAM roles and IAM users for supported resource types. You can use AWS RAM to share transit gateways, subnets, AWS License Manager license configurations, Amazon Route 53 Resolver rules.Not all resources can be shared.

## AWS STS
Web service allow you to request temporary, limited credentials for IAM users or for federated users(users managed by an external directory and granted temporary access to AWS services)
federated users use assumerolewithsaml on sts to gain 
web identity fedaration
assumerolewithwebidentity

