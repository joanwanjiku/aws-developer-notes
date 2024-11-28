# Cloudfront
- Content delivery service
- Improves read performance, content is cached at the edge
- Improves users experience, 216 point of presence globally(edge locations)
- DDos protection(becaues worldwide), integration with shield, AWS Web application firewall.

## Cloudfront origins
- **S3 bucket**;- for distributing files and caching them at the edge location using cloudfront
    - Enhanced security with cloudfront origin access control.
    - OAC() is replacing OAI, to restrict access to the content you serve from s3 buckets
    - Ingress:- upload content to S3 bucket
- **Custom Origin**:- 
    - Application Load balancer
    - EC2 instance
    - S3 website(bkt must be enabled for static website)
    - Any HTTP backend you want
- **ALB as Origin**
    - 
## CloudFront VS CRR
1. Cloudfront
    - Global Edge network, Files are cached for a TTL, Great for static content
2. CRR
    - Must be setup for each region you want it to happen, Files updated real time

## Cloudfront Caching & Caching policies
- The cache lives at each cloudfront edge location.
- Cloudfront identifies each  object in the cache using the cache key.
- **Cache Key**:- Unique identifier for each object in the cache, made up of hostname + resource portion of the url.
- You can add other elements to the cache key, if you have an app that serves content based on user, location, device e.t.c using cloudfront cache policies.
### Cache Policy
- Cache based on 
    1. HTTP Headers:- None, Whitelist
    2. Cookies:- (What values will be included in the cache key + forwaded to the origin) None, Whitelist, include all-except, all
    3. Query Strings:-(What values will be included in the cache key + forwaded to the origin)  None, Whitelist, include all-except, all
- Control the TTL
- Create own or use predifined.
- **Origin Request Policy**:- 

## Cloudfront geo restriction
- You can restrict who can access your distribution
    - Allowlist:- Allow users access content if they're in one of the listed countries
    - BlockList:- Prevent users access content if they're in one of the listed countries
- Use case:- Copyright laws to control access to content.

## Cache invalidations
- In case you update the backend origin, Cloudfront doesn't know about it, and will only get the refreshed content after the TTL has expired.
- However, you can force an entire or partial cache refresh (by bypassing the TTL).
- You can invalidate all files/or special path.

## Global Accelerator
- Leverages the AWS internal network to route to your application.
- 2 Anycast IP are created for your application, the Anycast IP send traffic directly to edge locations, The edge locations send the traffic to your application.
- Works with elastic IP, EC2 instances, ALB, NLB,
- Consistent performance;- intelligent routing to lowest latency and fast regional failover
- Health checks;- great for DR
- Security:- only external IP need to be whitelisted, DDos protection thanks to AWS Shield 

## Cloudfront Signed Cookies/Cookies
- You want to provide access to multiple restricted files
- You don't want to change your current URLs

## Cloudfront behaviours
- It is configuration within a distribution. Origins are directly linked to behaviours, behaviours are linked to distributions.
- Example, redirect /images/* to an S3 bucket

## Cloudfront Charges
1. Charge for storage in an amazon S3 bucket
2. Charge for serving objects from an edge location
3. Charge for submitting data to your origin server
create a distribution

create a link to your object

## steps
1. Under Origin, in the Origin Domain text-box, enter the Amazon S3 static website hosting endpoint
2. Select default root object:- You are setting this field because Amazon CloudFront doesn't always transparently relay requests to the origin. If you did not set a default root object on the distribution you would see an AccessDenied error when you access the CloudFront distribution's domain later.


