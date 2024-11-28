# EC2 Auto Scaling
With auto scaling, you can specify the **minimum, maximum, desired** number of instances in each auto scaling group. If you specify **scaling policies**, auto scaling launches or terminates new instances when demand of your application increases or decreases.

## 
## Auto Scaling Group
- An ASG maintains the number of instances by performing periodic health checks on the instances in the group. 
- ASG is free, pay for the ec2 resources used


## Launch template
Launch templates enable you to store launch parameters so that you do not have to specificy them every time you launch an instance. Launching an ec2 instance through AWS SDK, AWS CLI or EC2 console, you can specify the launch template to use. A launch template allows you to have multiple versions of a launch template. You can access the latest features and improvements
-  Mix of instance types

## Launch configurations
- You can specify your launch configuration with multiple Auto Scaling groups. However, you can only specify one launch configuration for an Auto Scaling group at a time, and you can't modify a launch configuration after you've created it. To change the launch configuration for an Auto Scaling group, you must create a launch configuration and then update your Auto Scaling group with it.
you cannot create an Auto Scaling group that launches both Spot and On-Demand Instances or that specifies multiple instance types. You must use a launch template to configure these features.

## Scaling plans
Tells auto scaling when and how to scale. Type of plans:- 
- **Maintain current evels at all times**:- auto scaling performs periodic health checks, if an instance is unhealthy, it is terminated and launches another one.
- **Manual scaling**:-you specify a change in the maximum, minimum, desired capacity of your auto scaling group.
- **Scaling based on a schedule**:- scaling actions are performed automatically as a function of time and date. Scheduled scaling
- **Scale based on demand**:- Define parameters that control the auto scaling process.
- **Dynamic Scaling**:- A dynamic scaling policy instructs Amazon EC2 Auto Scaling to track a specific CloudWatch metric, and it defines what action to take when the associated CloudWatch alarm is in ALARM
    1. Target tracking scale:- Increase or decrease the current capacity of the group based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home—you select a temperature and the thermostat does the rest.
    2. Step Scaling:- Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
    3. Simple scaling:- Increase or decrease the current capacity of the group based on a single scaling adjustment.
    4. Predictive Scaling:- based on a forecast, patterns that repeat themselves.

### Steps
1. Create a launch template. Under ec2 > launch templates > create > select **AMI** > **Instance type** > **SG**
2. Create an auto scaling group. Auto scaling groups > create > select launch template >**VPC** then subnets(multiple for HA), > attach **ELB**(optional), health check grace period, enable cloudwatch monitoring > group size,  > notifications > tags > review.
3. You can test the group through instance management tab and activity tab.

1. Create launch configuration > select AMI > select instance type > follow steps

# Auto-scaling and NLB
ec2 > load balancer > select a NLB > create a target group(if not created). The target's group protocol for NLB should TCP, in the Target type option allows you to specify IP addresses or a Lambda function in addition to an Amazon EC2 instance. Using an IP address gives you the ability to use a network load balancer with compute instances outside of AWS). The deregistration(in attributes tag) delay specifies how long the load balancer should wait before removing an instance from the target group > 
**Notice that you have created separate security groups for the instances and the load balancer. In a non-lab environment, it is often the case that the instances are listening on a different port than the load balancer, and the load balancer re-directs the traffic. Assigning separate security groups allows you to more flexibly control and restrict traffic at the network level.** 

## Creating an ASG
create an ASG from the launch template created, attach the ELB, set number of instances, 