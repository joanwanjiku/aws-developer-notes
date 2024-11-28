# IAM
## features of IAM
- Users and groups are global.
- KMS
1. Roles:- Allows users to adopt a set of temporary IAM permissions.
    ### Concepts
    - Role chaining:- 
    - Trust policy: define the principals you trust to assume a role.
    - Permissions policy:- define what actions and resources the role can use.
    - Permissions boundary:- feature that use policies to limit the maximum permissions that an identity-based policy can grant to a role.
    ### Types of roles
    - AWS Service Role:- used by services to assume a role to perform a task. set policy and assign permissions. e.g EC2 service-role is a role an EC2 instance can assume to perform actions on your account.
    - AWS service-linked role:-  is pre-defined, and can only be edited in a limited number of cases. Unique service role that is linked directly to an AWS service which is predefined by the service and includes all permissions that the service requires to call other AWS services.
    - Roles for cross account access:- access to accounts you own and a third-party aws accounts 
        - *Trusting* account:- has resources that need to be accessed.
        - *Trusted* Account:- has users that need to access resources in the trusting account.
    

2. Policies:- they are used to assign permissions, A Json document
    An explicit allow in an identity-based or resource-based policy overrides the default deny. If a permission boundary, SCP or session policy is present, it might override the allow.
    ```
    "statement": [
        {
            "SiD": unique identifier within the statement array
            "Effect: grant or restrict access for the previous defined action
            "Pricipal": specifies who get the permission
            "Action": api calls for different services, prefixed with associated aws service e.g "S3:DeleteBucket"
            "Resource": specifies the actual resource you wish the action and effect to be applied to. AWS uses ARNs to specify resources.
        }
    ]
    ```
    ### Types of policies
    - **Identity-based policies**: grant permissions to an identity

           1. managed policies: standalone polices, attached to multiple users, groups,
           aws-managed policies, customer-managed policies. Inherited from a group.
                
           2. inline policies:- added directly to a single user, group, role, which maintain a strict one-to-one relationship between a policy and an identity and deleted when you delete an identity.
    - **Resource-based policies**: attach inline polices to resources, grant permissions to the principal that is specified in the policy. No managed polices
    - **Permission Boundaries**: This use managed policies as the boundary for an IAM entity, which defines the maximum permissions that the identity-based policies can grant to an entity.
    - **Organization SCPs**: use SCP to define the maximum permissions for account members of an organization or oragizational unit.
    - **ACL**:- control which principals in other accounts can access a resource to which the ACL is attached.
    - **Session Polices**: limit permissions that the role or user's identity-based policies grant to the session, limit permissions for a created session, but do not grant permissions.

3. Users:- Objects representing an identity
4. Groups:- Used to authorize access through policies
5. Identity federation: allows you to access aws resources even if you do not have a user account with IAM eg microsoft active directory, openID connect. Allows for SSO without creating multiple users
    - OpenID
    - saml v2
## Cross-account access
Allows users from one aws account to access services within a diffirent aws account through the use of IAM roles.

    - *Trusting* account:- has resources that need to be accessed.
    - *Trusted* Account:- has users that need to access resources in the trusting account.
    ### Steps
    Firstly, you must create a role from within the trusting account, which would be the production account in our example. This is to establish a trust between the two accounts. This role will define the development account as a trusted entity. Next, you must specify the permissions attached to this newly created role, which the users in the development account would assume to carry out their required actions and tasks. Next, you must switch to the trusted account--in this scenario the development account--to grant permissions to your developers to allow them to assume the newly created role in the trusted account. Finally, you can test the configuration by switching to the role.

# Security tools
- IAM Credentials report(account-level):- a report that lists all your account's users and the status of their various credentials.
- IAM Access advisor(user-level):- access advisor shows the service permissions granted to a user and when those services were last accessed.

## Labs
### Creating a user group
1. On IAM, Groups Tab, click create group
2. Choose a group name, attach policies, you can attach upto 10 policies
### Creating a user
1. Values:-User name(Name must be case sensitive), Access type
2. Add permissions > then next tags > review > create user
3. Download CSV file containing security credentials.
### Logging in  with the new IAM cred
- try an action other than the assigned
### Creating a policy
- Under policy tab, create a policy, add permissions
### Attaching a policy
- Search for policy > actions > attach > select user/group
### Creating  a role
- Roles > create role > select type of trusted entity > add permissions > tags > role-name
### Launch ec2 instance with IAM Role
- Under instances > launch instance > under configuration details, on IAM role, select the iam role to attach > launch

