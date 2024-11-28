# EFS
- managed network file system that can be mounted on multiple EC2 instances in diff AZs.
- Content management, data sharing, web serving.
- Uses SG to control access
- Compatible with linux based AMIs(not windows)
- **Scale**;- 1000s of concurrent NFS clients, 10GB+ throughput

## Performance modes
- General purpose:- latency senstive use cases
- Max I/O:- higher latency, throughput, highly parallel(big data, media processing)

## Throughput mode
- Bursting mode:- 1TB = 50mibs transfer speed + burst to 100MiBs
- Provisioned/enhanced

## Storage classes
- Move files between the tier with lifecycle policies
- Standard:- for frequently accessed files.
- EFS-IA:- cost to retrieve files, lower price to store. 

## Performance and storage classes
1. **EFS Scale**
- 1000s concurrent NFS clients.
- grow to petabyte-scale nfs, automatically.
2. **Performance mode**
    - General purpose:- latency-sensitive cases (web server, cms).
    - Max I/O:- higher latency, throughput(big data, media processing).
3. **Throughput Mode**
    - Bursting:- 
    - Provisioned:- set throughput regardless of storage size.
## Creating an efs
Before creating a file make sure your account is allowed, attach a policy e.g 
``` 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToCreateEFSFileSystem",
            "Effect": "Allow",
            "Action": [
                "elasticfilesystem:CreateFileSystem",
                "elasticfilesystem:CreateMountTarget"
            ],
            "Resource": "arn:aws:elasticfilesystem:region-id:file-system/*"
        },
        {
            "Sid": "PermissionsRequiredForEC2",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeSubnets",
                "ec2:CreateNetworkInterface",
                "ec2:DescribeNetworkInterfaces"
            ],
            "Resource": "*"
        }
    ]
}
 ```

Additional permissions
```
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1AddtionalEC2PermissionsForConsole",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpcAttribute"
            ],
            "Resource": "*"
        }
    {
            "Sid": "Stmt2AdditionalKMSPermissionsForConsole",
            "Effect": "Allow",
            "Action": [
                "kms:ListAliases",
                "kms:DescribeKey"
            ],
            "Resource": "*"
        }
    ]
}
 ```
- Have intances in a subnet, in a vpc same as the file system NB:To allow ssh into the instances in the public subent, the IG must be added in the VPC's route table, instances' SG must allow inbound traffic.
- Add instances SG-ID to the efs SG with protocol as NFS.
- While creating a FS, in mount targets, select all AZs with instances and filesystem-SG
- install mount-helper into the instances, mkdir of folder to mount the efs, using the efs-id mount the file system to the directory.

## AWS Backup
AWS Backup acts as a central hub to manage backups across your environment, across multiple regions, centralizing management and providing full auditability in addition to assisting with specific compliance controls. Having a managed service monitor and control your backups allows for all logging to be consolidated in a single place, in addition to seeing the status of completed backups and perform and restores required.


