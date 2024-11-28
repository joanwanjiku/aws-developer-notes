# KMS
Managed service, to create and manage encyrption keys.Creates and stores master keys called CMKs, CMKs are protected with **FIPS 140-2**
## Customer Master Keys (CMK) - 
- The primary resources in AWS KMS are customer master keys (CMKs).
- Typically you use CMKs to protect data encryption keys (or data keys) which are then used to encrypt or decrypt larger amounts of data outside of the service. 
- CMKs never leave AWS KMS unencrypted, but data keys can. AWS KMS does not store, manage, or track your data keys. 
- There is one AWS-managed CMK for each service that is integrated with AWS KMS. When you create an encrypted resource in these services, you can choose to protect that resource under the AWS-managed CMK for that service. This CMK is unique to your AWS account and the AWS region in which it is used, and it protects the data keys used by the AWS services to protect your data. Encrypt upto 4kb of data.
    - AWS managed CMKs
    - AWS owned CMKs
    - Customer managed CMKs

- Data keys - Data keys are used encrypt large data objects within an application outside AWS KMS. In the context of S3 server-side encryption using KMS keys, the application is S3 itself.

Key rotation and Backing Keys - When you create a customer master key (CMK) in AWS KMS, the service creates a key ID for the CMK and key material referred to as a backing key that is tied to the key ID of the CMK. If you choose to enable key rotation for a given CMK, AWS KMS will create a new version of the backing key for each rotation. It is the backing key that is used to perform cryptographic operations such as encryption and decryption. Automated key rotation currently retains all prior backing keys so that decryption of encrypted data can take place transparently. CMK is simply a logical resource that does not change regardless of whether or of how many times the underlying backing keys have been rotated.

# CloudHSM
A HSM is a computing device that processes cryptographic operations and provides secure storage for cryptographic keys. CloudHSM provides HSMs in acluster which you can create with your own VPC, Comply with **FIPS 140-2 Level 3**. Comply with PCI DSS

# SAML 2.0 Based federation
To use SAML 2.0 federation the organization's Idp and AWS account must be configured to trust each other.
AWS supports identity federation with the open SAML 2.0, used by many identity providers
Allows for federated SSO. 
Users can log into the console, call AWS API operations, without needing an IAM user for every user in the organization.
Simplifys process of configuring federation with AWS by using Idp's, instead of writing custom identity proxy code using the GetFederationToken API operation.
    -- Use cases
    federated access allowing an org. user/app to call AWS API operations.
    Web-based single sign on(SSO) to the AWS management console from organization- users sign in an organisation portal hosted by a SAML 2.0-compatible IdP then are redirected to the console without having to provide additional
    ![Saml federation access](images/saml_federated_acces.png)

# SAML 2.0 federation - active directory federation service
simplify administration of user access in AWS using a federated solution with an external directory.leverages 
![Saml federation process](images/saml_federation_process.png)

## Lab
1. On KMS, create key, select key type > add labels, append a unique number to the key's alias, if needed to be unique.> next, Define key administrative permissions > Define key usage permissions
* **Administrative permissions** allow users and roles to administer CMKs but not to perform cryptographic operations. In production environments, this is sometimes used to easily grant limited access to other users. The Allow key administrators to delete this key checkbox makes it explicit if deleting keys is allowed since the key can not be recovered once deleted, making recovery of encrypted data impossible. Note that key deletion is not immediate and first enters into a pending state before the key is deleted. The delete operation can be canceled while in the pending state. 
* **Usage permissions** grant access to perform cryptographic operations such as encrypting and decrypting. Enterprises usually have different permissions for administrators and users, hence the wizard walks you through defining both.

2. **Encrypting S3 data using SSE-KMS**:- 
While uploading the object, on properties tab select SSE > enable > SSE-KMS > choose from AWS KMS keys > select your key.

3. **Enforcing S3 Encryption Using Bucket Policies**:- 
To ensure all data uploaded to the bucket is encrypted, you implement bucket policies that require SSE to be used on all objects.N ote: The policy should be enforced when the bucket is created since existing objects in a bucket will not be encrypted when the policy is applied.
In permissions > bucket policy > edit policy
` {
    "Version": "2012-10-17",
    "Id": "RequireSSEKMS",
    "Statement": [
        {
            "Sid": "DenyUploadIfNotSSEKMSEncrypted",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<Your_Bucket_Name>/*",
            "Condition": {
                "StringNotEquals": {
                    "s3:x-amz-server-side-encryption": "aws:kms"
                }
            }
        }
    ]
}  ` *This policy denies ("Effect": "Deny") all users' ("Principal": "*"*) uploads ("Action": "s3:PutObject") to the bucket ("Resource": "arn:aws:s3:::<Your_Bucket_Name>/*"*) if the s3:x-amz-server-side-encryption is not set to aws:kms, which corresponds to SSE-KMS.*


