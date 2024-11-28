# AWS Systems Session Manager
- Session Manager provides secure and auditable node management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.

## CLI command that triggers run-command 
- `aws ssm send-command --document-name "c32617a413336l905256t1w511742835724-InstallDashboardApp-bkKho3HnZqrd" --document-version "1" --targets '[{"Key":"InstanceIds","Values":["i-084297d9a0df4e77d"]}]' --parameters '{}' --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region us-west-2`

## Launch EC2 instance using CLI
Use the AWS Systems Manager Parameter Store to obtain the ID of the most recent Amazon Linux 2 AMI. 
AWS maintains a list of standard AMIs in the Parameter Store, making this task easy to automate.
- Set the Region
    - AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
    - export AWS_DEFAULT_REGION=${AZ::-1}

- Obtain latest Linux AMI
    - `AMI=$(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --query 'Parameters[0].[Value]' --output text)`
    - echo $AMI

- Obtain a subnet tagged Name and value is public subnet
    - `SUBNET=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=Public Subnet' --query Subnets[].SubnetId --output text)`
    - echo $SUBNET

- Obtain a sg named webSecurityGroup
    - `SG=$(aws ec2 describe-security-groups --filters Name=group-name,Values=WebSecurityGroup --query SecurityGroups[].GroupId --output text)`
    - echo $SG

- Add user data:- add commands into a text file

- launch an Instance
    - `INSTANCE= $(\
    aws ec2 run-instances \
    --image-id $AMI \
    --subnet-id $SUBNET \
    --security-group-ids $SG \
    --user-data file:///home/ec2-user/UserData.txt \
    --instance-type t3.micro \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Web Server}]' \
    --query 'Instances[*].InstanceId' \ The query parameter specifies that the command should return the Instance ID once the instance is launched.
    --output text \
    )`
    - echo $INSTANCE


## Creating a vpc
    -
## Connecting to a instance within the private subnet
1. log into the instance within the public subnet
2. sudo su ;- root user
3. create a .pem file --> should be named as the key-pair used while creating the instance in the private subnet
4. copy the contects of the downloaded .pem file into the .pem created above.
5. make the .pem executable
6. ssh into private instance NOW!!!
7. to ensure that the private subnet can traffic out, check for a nat-gateway in it's route table that sits in the public subnet

## Creating a load balancer
1. Creating an ELB 
    - `aws elbv2 create-load-balancer --name <elb-name> --subnets <subid> --security-groups <sgid>`
2. Creating a target-group
    - `aws elbv2 create-target-group --name <tg-name> --protocol HTTP --port 80 --vpc-id <vpc>`
3. Register EC2 instances to the target group
    - `aws elbv2 register-targets target-group-arn targetgroup-arn --targets id=<id>`
4. Create a listener for the elb
    - `aws elbv2 create-listener  --load-balancer-arn loadbalancer-arn --protocol HTTP --port 80 -default-actions Type=forward, TargetGroupArn=targetgroup-arn`
5. Register EC2 instances to the target group
    - `aws elbv2 describe-target-health --target-group-arn targetgroup-arn`

