# Cloudformation
- Declarative way of outlining your AWS infrastructure.
- Infrastructure as code:- 
## CloudFormation works
- Template must be uploaded in s# and the references in cloudFormation.
- You can't edit a template, upload new versions.
- Stacks are identified by a name, deleting stacks, deletes every artufact that was created by cloudformation.

## Components
1. AWSTemplateFormatVersion
2. Description
3. **Resources**:- Resources can reference each other, they represent AWS components that will be created and configured
    - configuration `service-provider::service-name::data-type-name` .
    - refer to "[aws resources reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)"
    - 
4. **Parameters**:- A way to provide input to your cloudformation template, when you want to reuse, if the resource configuration is likely to change in future. Can be controlled by Type, Description, e.g 
     ```yaml
      Parameters:
        InstanceType:
            Description: choose ec2 type
            Type: String
            AllowedValues:
                - t2.micro
                - t2.small
            Default: t2.small 
     ```
    - Use !Ref to reference parameters and other elements in the template
5. **Mappings**:- Fixed variables within your cloudformation template, come in handy to separate envs and aws regions. e.g
    ```yaml
    Mappings:
        RegionMap:
            us-east-1:
                HVM64:ami-geugud7728
            us-west-1:
                HVM64:ami-2772bbvet3

    Resources:
        MyEC2Inst:
            Type:AWS::EC2::Instance
            Properties:
                ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
    ```
6. **Outputs**:- Best way to perform collaboration between stacks and cloudformation templates.
7. **Conditionals**:- Allow Creation of resources based on a condition. can be applied to outputs.

### Intrinsic functiona
- !Ref:- reference
- !GetAtt:- 
- !FindInMap
- !Base64:- pass encoded data e.g user data in EC2

### Rollbacks
- If stack fails during creation:- everything rollsback(gets deleted).
- If stack fails during updating:- everything rollsback to the previous version.

### CLoudFormation Service Roles
- IAM role that allows cloudformtaion to create/updaye/delete stack resource on your behalf.
- Want to invoke principle of least privilege.

###  Cloudformation capabilities
- **CAPABILITY_NAMED_IAM and CAPABILITY_IAM**:- Necessary to enable when your cloudformation template is creating or updating IAM resources.

### Deletion Policy
- Dictates what happens when a template is deleted, Safety for preserve and backup resources

#### Custom resources
- Define resources not supported by cloudformation
- Define custom provisioning logic for resources that are outside cloudformation(on-prem resources)
- When you want custom scripts to run during create/update/delete through lambda funcs
- Use Cases:- delete content in s3 bkt.

#### Stacksets
- Create, update or delete stacks across multiple accs and regions with a single operation.
- Applied to an AWS organization
- Admin acc can create stacksets