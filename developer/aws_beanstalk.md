# Elastic Beanstalk
- Develop centric view of deploying an application on AWS.
- Managed service, handles capacity provisiong, load balancing, scaling, app health monitoring, instance configuration.
- Can create multiple envs.
- Service is Free but pay for underlying resources.

## Components
1. Application:- collection of Elastic Beanstalk components(Envs, versions, configurations)
2. Application Version:- iteration of your application code.
3. Environment:- Collection of AWS resources running an application version.
- **Web Server vs Worker Tier**:- 
 ### Deployment mode
- **Single Instance**:- dev work, 1 ec2 instance with elastic IP
- **High availability with load balancer**:- Prod env, multiple ec2 instances, multiAZ

### Deployment Modes
1. **All at Once**:- fastest deployment, Great for quick iterations in development environment. No additional cost
2. **Rolling**:- App is running below capacity, can set the bucket size. App is running simultaenously, ni additional cost
3. **Rolling with additional batches**:- App is running at capacity, small additional cost, longer deployment, good for prod.
4. **Immutable**:- Zero downtime, new code deployed to new instances on a temp ASG, longest deployment, quick rollback, great for prod
5. **Blue?Green**:- create a new 'stage' env to deploy v2, the new env can be validated independently and roll back if issues
6.**Traffic Splitting**:- Canary testing, New app is deployed on a temp ASG, test using small % of traffic sent to the temp ASG

### Beanstalk lifecycle
- Eb can store upto 1000 app versions
- 

### Beanstalk extensions
-  
#### Beanstalk cloning

#### Beanstalk Migration
- After creating an elastic beanstalk env, you cannot change the Elastic load balancer type(Only the configuration).
- Decouple RDS
- 
