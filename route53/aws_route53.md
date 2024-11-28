# DNS Route53
- Fully managed, HA, scalable and authoritative DNS
- Domain register.
- 100% availability SLA
- Determines how you want to route traffic for a domain.
- Each record contains
    - Domain/subdomain Name:- 
    - RecordType
    - Value
    - Routing policy
    - TTL:- clients cache the results of DNS, 

## Record Types
1. **A** :- maps a hostname to IPv4
2. **AAAA**:- maps a hostname to IPv6
3. **CNAME**:- maps a hostname to another hostname. 
                target is a domain name which must have a A or AAAA Record. Cannot create CNAME record for the top node of a DNS namespace( root Domain e.g mydomain.com)
4. **NS**:- name server for the domain server, control how traffic is routed for a domain.
5. **ALIAS**:- Maps hostname to AWS resource, can be used for the top node of a DNS record, recognizes changes in resource IP addresses, 
                Alias record is always   of type A/AAAA. cannot set TTL. Cannot set ALIAS record for EC2 DNS name.
                Targets:- ELB, API gateways, S3 websites, VPC interface endpoints, Cloudfront distribution, elastic beanstalk. Free to query

## Hosted Zone
- A container for records that define how to route traffic to a domain and its subdomains
- Public hosted zone:- contains records that specify how to route traffic on the internet
- Private hosted zone:- contains records that specify how to route traffic within a private VPC


### Routing Policy
#### Simple
- Route traffic to a single resource.
- Can specify multiple values in the same record.
- Cannot be associated with health checks
- 

#### Weighted
- Control the % of the requests that go to each specific resource
- Assign each record a relative weight.
- DNS records must have the same name and type.
- Can be associated with heath checks.
- Use cases:- load balancing between new application versions

#### Latency
- Redirect to the resource that has the least latency.
- Use cases:- latency for users is a priority.
- Can be associated with health checks

#### Health checks
- Health check => automated DNS failover
    1. health checks that monitor an endpoint, 
        About 15 global health checkers will check the endpoint health.
        Health checks pass when status code is 2XX or 3XX
        Configure your router/firewall to allow incoming request from Route53 health checkers.
        Route53 health checks are outside the VPC.
    2. Health checks that monitor cloudwatch alarms.
        Can create a metric then associate it with an alarm, then create a check that checks the alarm itself, since health checks are outside a vpn and cannot access private endpoints.
    3. Health checks that monitor other health checks. Calculated health check

#### Failover
- Revereges on health checks to reroute traffic to healthy resouce

#### Geolocation
- Routing based on the location of the user.
- Restrict content distribution, load balancing, website localization
- Should have a default page for no match found

#### Geoproximity
- Route traffic to users based on geographic location of users and resources.
- Use a bias:- Ability to shift more traffic to resources based on defined bias
- Changing the size of the geographic region, specify bias values.
    - Expand(more traffic to the resource)
    - Shrink(less traffic to the resource)
- **Traffic policies** visualizes 

#### Multivalue
- Can be associated with health checks
- When routing traffic to multiple resources.
- Upto 8 healthy records are returned for each multi-value query

#### IP-Based
-  Routing based on clients IP
- Provide a CIDR block, route to a specific endpoint