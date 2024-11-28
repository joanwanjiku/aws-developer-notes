# AMI
- You can customise an instance, e,g install necessary sw then create an AMI from the instance
- You can create an instance store-based ami or an EBS volume-based AMI
- built for specific region, can be copied cross-region

## create an AMI
select instance > actions > image > create image

## Copying an AMI
prerequisite: create or get an AMI backed by an ebs snapshot.
create an AMI: select an EBS-backed AMI as the source for your new AMI copy.
launch an instance of the source EBS-backed AMI