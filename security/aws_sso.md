# Single Sign On
Simple management of all identities
supports MFA
Intergrates with several 3rd party SAAS providers
AWS SSO works as an identity provider (IdP) for any SAML 2.0–compliant cloud applications.

Aws organizations, an organization has to be created
When configuring SSO for an application e.g dropbox,  To configure this application for SSO access, you must establish a trust relationship between AWS SSO and your cloud application (service provider) through a SAML metadata exchange.

# Active Directory services
Multiple approaches to use microsoft AD with other AWS services
Directories store user, group , device information
## AWS managed microsoft AD
AWS Managed Microsoft AD enables you to sign in to the AWS Management Console. With AWS Single Sign-On, you can also obtain short-term credentials for use with the AWS SDK and CLI, and use preconfigured SAML integrations to sign in to many cloud applications. By adding Azure AD Connect, and optionally Active Directory Federation Service (AD FS), you can sign in to Microsoft Office 365 and other cloud applications with credentials stored in AWS Managed Microsoft AD. 
- *Best choice if you need actual Active Directory features to support AWS applications or Windows workloads, including Amazon Relational Database Service for Microsoft SQL Server. It's also best if you want a standalone AD in the AWS Cloud that supports Office 365 or you need an LDAP directory to support your Linux applications.*
    - Standard Edition: AWS Managed Microsoft AD (Standard Edition) is optimized to be a primary directory for small and midsize businesses with up to 5,000 employees. It provides you enough storage capacity to support up to 30,000* directory objects, such as users, groups, and computers.
    - Enterprise Edition: AWS Managed Microsoft AD (Enterprise Edition) is designed to support enterprise organizations with up to 500,000* directory objects.
Provides microsoft active directory in the AWS cloud which supports:
    1. Active Directory-aware workloads
    2. RDS for microsoft SQL server
    3. AwS managed services e.g workspaces, quicksight
    4. linux applications that use an LDAP directory

![AD Connector](images/Aws_managed_microsoft_AD.png)


## AD Connector
Proxy service that connects aws applications e.g workspaces, quicksight and windows EC2 instances to existing on-premises microsoft AD. With AD Connector , you can simply add one service account to your Active Directory. AD Connector also eliminates the need of directory synchronization or the cost and complexity of hosting a federation infrastructure.
- *AD Connector is your best choice when you want to use your existing on-premises directory with compatible AWS services.*
![AD Connector](images/AD_connector.png)


## Simple AD
Simple AD is a Microsoft Active Directory–compatible directory from AWS Directory Service that is powered by Samba 4. Simple AD supports basic Active Directory features such as user accounts, group memberships, joining a Linux domain or Windows based EC2 instances, Kerberos-based SSO, and group policies.Simple AD is a standalone directory in the cloud, where you create and manage user identities and manage access to applications. You can use many familiar Active Directory–aware applications and tools that require basic Active Directory features. Simple AD does not support multi-factor authentication (MFA), trust relationships, DNS dynamic update, schema extensions, communication over LDAPS, PowerShell AD cmdlets, or FSMO role transfer.
- *You can use Simple AD as a standalone directory in the cloud to support Windows workloads that need basic AD features, compatible AWS applications, or to support Linux workloads that need LDAP service.*

## AWS Cognito
Amazon Cognito is a user directory that adds sign-up and sign-in to your mobile app or web application using Amazon Cognito User Pools. You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory. This fully managed service scales to support hundreds of millions of users
User pools:- User directories that enable sign-up and sign-in options for app users through cognito or third-party IDps. Can use lambda triggers to customize workflows and user migration, MFA, 
![User pool](images/user_pool.png)
Identity pools:- use to grant users to other aws resources. Supports anonymus geust users and identity providers that can be used to authenticate users for identity pools
- *You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory. This fully managed service scales to support hundreds of millions of users. Need a fully managed service that scales to support hundreds of millions of users*

## Cognito sync
Service and client library that provides cross-device syncing of application user data

## AWS Secrets manager
Used to replace hard-coded credentials e.g passwords, with an API call to secret manager to retrieve the credentials programmatically. 
Configure SM to rotate secrets according to a defined schedule using a lambda function. Encrypts data using KMS key.
Provide only the secret name or ARN and SM returns the result value of the secret.Deleting secrets takes a 7 day waiting period.

## AWS GuardDuty
Aws managed service that uses machine learning, threat intelligence feeds to identify unexpected and potentially unauthorized and malicious activity within aws env. Contionous security monitoring service that analyzes and processes the VPC flow log, cloudtrail management event log, cloudtrail S3 data event log and DNS data sources.
Can detect compromised EC2 instances, DDos network attack. Monitos AWS account access behavior for signs of compromise.
Stores findings for 90 days,
Status reports through cloudwatch events
- GuardDuty S3 protection:- monitors object-level API calls, identifyong potential threats. Analyzes cloudtrail management events and cloudtrail s3 Data events
![Guard Duty](images/guardDuty.png)

### lab
Enable guarduty in your region(30-day trial period) >generate findings> store them in an S3 bucket >configure findings alert through SNS Topic >Eventbridge rule to format the findings.
![Guard Duty Lab](images/guardduty.png)

## Amazon Inspector
Tests network accesiblity of EC2 instances, and the security state of applications running on those instances.
Assesses applications for exposure, vulnerabilities and deviations from best practises.
Automate security vulnerabilty assessments throughout development and deployment pipelines as well as static production systems.
![Inspector pricing](images/inspec.png)


## Amazon Macie
Fully managed data security and data privacy service, Uses machine learning, pattern matching to discover exposure of PII data, Identify buckets with excess permissions.
Can publish events to eventbridge
![Macie](images/macie.png)



