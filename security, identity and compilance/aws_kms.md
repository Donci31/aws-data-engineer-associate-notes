# AWS Key Management Service (KMS)

AWS Key Management Service (KMS) is a fully managed encryption service that allows you to create, manage, and control cryptographic keys used to secure your data. KMS integrates seamlessly with many AWS services, ensuring strong encryption at rest and in transit, while aligning with compliance requirements like HIPAA.

## **Key Features of AWS KMS**
- **Centralized Key Management**: Manage cryptographic keys securely with granular access controls.
- **Integration with AWS Services**: Works with services such as EBS, S3, RDS, and more for seamless encryption.
- **CloudTrail Integration**: Enables auditing of key usage and access attempts.
- **Flexible API**: Encrypt, decrypt, and manage keys programmatically using AWS SDK or CLI.
- **Never Exposes Plaintext Keys**: All operations involving cryptographic keys are securely handled within AWS.

## **KMS Key Types**
1. **Symmetric Keys**:
   - AES-256 keys for both encryption and decryption.
   - Integrated with AWS services for most encryption use cases.
   - Key is never directly accessible; operations must call the KMS API.
2. **Asymmetric Keys**:
   - RSA or ECC key pairs for encryption/decryption or digital signature verification.
   - Public key is downloadable for use outside AWS, but private key remains secure.
   - Use cases include cross-platform encryption where KMS API calls are impractical.

## **Types of KMS Keys**
1. **AWS-Owned Keys**:
   - Managed entirely by AWS and used by default (e.g., SSE-S3, SSE-SQS).
   - Free of charge.
2. **AWS-Managed Keys**:
   - Automatically created by AWS for specific services (e.g., `aws/rds`, `aws/ebs`).
   - Free of charge but limited to specific AWS services.
   - Automatically rotated every year.
3. **Customer-Managed Keys**:
   - Fully customizable and allows precise access control.
   - $1 per month per key + API call charges.
   - Can be configured for automatic or on-demand rotation.
   - Supports features like cross-account access.
4. **Imported Keys**:
   - Bring your own keys to AWS for integration.
   - $1 per month per key; rotation must be manual using an alias.

## **KMS Key Policies**
Key policies in KMS define access controls for KMS keys and are required to grant permissions. They function similarly to S3 bucket policies but are mandatory.

### Types of Key Policies:
1. **Default Key Policy**:
   - Suitable for simple scenarios where only the account owner or administrators need access to the key and no cross-account sharing or detailed access restrictions are required
2. **Custom Key Policy**:
   - Allows granular control over access and administration.
   - Necessary for enabling **cross-account access** or restricting specific operations.

> KMS does not physically share keys across accounts. Instead, access to key usage is granted through policies.

## **Cross-Account Access with KMS**
To securely share encrypted data between AWS accounts:
1. **Create a Snapshot** encrypted with your customer-managed key.
2. **Attach a Key Policy** to allow access to the target account.
3. **Share the Snapshot** with the target account.
4. In the target account, **copy the Snapshot**, re-encrypting it with a key in that account.
5. **Create a Volume** from the copied snapshot for use.

This process ensures that the encryption key remains within its originating account while enabling secure data sharing. The shared key is used for decryption when copying the snapshot.

## **Security and Compliance**
AWS KMS provides robust security measures for key management:
- **HIPAA Compliance**: Aligns with encryption requirements.
- **Encrypted Secrets**: Never store sensitive data, such as passwords, in plaintext.
- **Auditing**: Use CloudTrail to track and monitor key usage.

## **Use Cases**
1. **Encryption in AWS Services**: KMS encrypts data for services like S3, EBS, and RDS.
2. **Secure API Operations**: Use KMS APIs for encrypting and decrypting data programmatically.
3. **Cross-Account Sharing**: Enable secure collaboration while keeping encryption keys within their source account.
4. **External Encryption**: Utilize asymmetric keys for external systems that cannot directly access KMS APIs.

## **Best Practices**
1. **Enable Key Rotation**: Automatically rotate customer-managed keys for better security.
2. **Use IAM and Key Policies**: Combine IAM roles and key policies for layered access control.
3. **Audit Regularly**: Review CloudTrail logs to detect unauthorized access.
4. **Encrypt Secrets**: Always encrypt sensitive information and never store plaintext secrets.