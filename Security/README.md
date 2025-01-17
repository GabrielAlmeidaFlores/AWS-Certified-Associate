## Policy Interpretation

### Example 1

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::holidaygifts/*"
    },
    {
      "Effect": "Deny",
      "Action": ["s3:GetObject", "s3:GetObjectAcl"],
      "Resource": "arn:aws:s3:::holidaygifts/*",
      "Condition": {
        "DateGreaterThan": { "aws:CurrentTime": "2022-12-01T00:00:00Z" },
        "DateLessThan": { "aws:CurrentTime": "2022-12-25T06:00:00Z" }
      }
    }
  ]
}
```

Users can upload, delete, and manage objects in the `holidaygifts` S3 bucket at all times. However, between December 1, 2022, and December 25, 2022, at 6:00 AM UTC, they are explicitly denied the ability to read or view objects in the bucket. Outside of this time frame, they have full access as permitted.

Permissions:

- **Allow**: Users can upload, read, and delete objects in the holidaygifts bucket (`PutObject`, `GetObject`, `DeleteObject`, and their ACLs).

- **Deny**: During a specific time frame (from December 1, 2022, to December 25, 2022, at 6:00 AM UTC), users are explicitly denied the ability to read objects (`GetObject` and `GetObjectAcl`).

### Example 2

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyNonApprovedRegions",
      "Effect": "Deny",
      "NotAction": ["cloudfront:*", "iam:*", "route53:*", "support:*"],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": ["ap-southeast-2", "eu-west-1"]
        }
      }
    }
  ]
}
```

Users are denied access to most AWS services if the request comes from a region other than `ap-southeast-2` (Sydney) or `eu-west-1` (Ireland). However, global services such as CloudFront, IAM, Route 53, and Support are exempt from this restriction and remain accessible from any region.

Permissions:

- **Deny**: For all actions except services like CloudFront, IAM, Route 53, and Support, access is denied unless the requested region is ap-southeast-2 (Sydney) or eu-west-1 (Ireland).

### Example 3

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::cl-animals4life",
      "Condition": {
        "StringLike": {
          "s3:prefix": ["", "home/", "home/${aws:username}/*"]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::cl-animals4life/home/${aws:username}",
        "arn:aws:s3:::cl-animals4life/home/${aws:username}/*"
      ]
    }
  ]
}
```

Users can view all S3 buckets and their locations. In the `cl-animals4life` bucket, they can list objects but only under the `home/` prefix or their personal folder (`home/${aws:username}`). They also have full permissions, including upload, download, and delete, for their specific personal folder (`home/${aws:username}`).

Permissions:

- **Allow**: List all buckets (`ListAllMyBuckets`) and get their locations.
- **Allow**: List objects in the `cl-animals4life` bucket, but only for specific prefixes (`home/`, `home/${aws:username}/*`, or no prefix).
- **Allow**: Full access (`s3:*`) to the user's personal folder (`home/${aws:username}`) and its contents.

## AWS CloudHSM (Hardware Security Module)

CloudHSM is a hardware security module (HSM) service provided by AWS that offers a secure and dedicated hardware solution for managing and protecting cryptographic keys. Here's a breakdown of its key features and functionalities:

- **Single-Tenant HSM**: Unlike AWS Key Management Service (KMS), which is a shared but logically separated service, CloudHSM provides a true single-tenant hardware solution, ensuring complete isolation for your cryptographic operations.

- **AWS-Managed but Customer-Controlled**: AWS provisions the HSMs, but they are fully managed by the customer, giving complete control over cryptographic operations and the key material.

- **FIPS 140-2 Compliance**: CloudHSM is fully compliant with FIPS 140-2 Level 3, a stringent security standard for cryptographic modules. In contrast, KMS is certified at Level 2 overall, with some Level 3 components.

- **Industry-Standard APIs**: CloudHSM supports standard cryptographic libraries such as PKCS#11, Java Cryptography Extensions (JCE), and Microsoft CryptoNG (CNG), enabling seamless integration with various applications.

- **KMS Integration**: CloudHSM can be used as a custom key store for KMS, combining the benefits of KMS’s native integration with the security of CloudHSM.

- **Not High-Available**: Unlike KMS, CloudHSM does not offer built-in high availability. You need to configure redundancy and clustering for resilience.

- **Network Integration**: The HSMs operate in an AWS-managed HSM Virtual Private Cloud (VPC), but interfaces are added to your customer-managed VPC for secure communication.

- **AWS Has No Access to Secure Areas**: While AWS provisions the HSMs, it does not have access to the secure area where your cryptographic key material is stored.

- **No Native AWS Integration**: CloudHSM does not natively integrate with AWS services like S3 for server-side encryption (SSE). You must manually handle these integrations.

CloudHSM is ideal for organizations needing strong cryptographic control, compliance with strict security standards, and integration with custom applications while maintaining full responsibility for the HSM’s operation and redundancy.

## AWS KMS (Key Management Service)

AWS KMS is a regional, public service designed to securely create, store, and manage cryptographic keys. It supports both symmetric keys (a single key for encryption and decryption) and asymmetric keys (a public/private key pair). You can use KMS for a range of cryptographic operations, such as encrypting data, decrypting it, and managing digital signatures.

KMS ensures the keys never leave the service, which makes it highly secure and compliant with standards like FIPS 140-2 Level 2.

Key features of KMS:

- **Logical Keys**: A KMS key is represented as a logical entity containing metadata such as:

  - **Key ID**: A unique identifier for the key.
  - **Creation Date**: When the key was created.
  - **Policy**: Permissions defining who can use or manage the key.
  - **Description**: Information about the key's purpose.
  - **State**: The status of the key (enabled, disabled, or pending deletion).

- **Physical Material**: Backed by secure key material, which can be either:

  - Generated within KMS for maximum security.
  - Imported by the user to meet specific compliance or interoperability needs.

- **Key Size Limitations**: KMS keys can directly encrypt data up to 4 KB. For larger data, a `Data Encryption Key (DEK)` is used.

- **Regional Isolation**: Each key is tied to a specific AWS region and never leaves it, ensuring local compliance and performance.

- **Ownership Types**:

  - **AWS-Managed Keys**: Automatically managed by AWS for simplicity.
  - **Customer-Managed Keys**: Provide more customization, including the ability to define policies, enable key rotation, and control lifecycle.

- **Key Rotation**: Customer-managed keys can be configured for automatic rotation every year, creating a new backing key while retaining access to previously encrypted data.

### Key Policies and Security

KMS uses key policies to control access to encryption keys. These policies are:

- **Resource-Based**: Directly attached to the key, defining who can use or manage it.
- **Explicit**: Access must be explicitly allowed; even the AWS account owner isn’t granted implicit access.

Every KMS key requires a key policy to function. For example, a policy might allow specific IAM roles or users to:

- Use the key for encryption/decryption.
- Rotate the key.
- Schedule deletion.

Policies are written in JSON and support conditions for fine-grained access control. Here is an example of a KMS resource policy:

```json
{
  "Id": "key-consolepolicy-3",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::879381281606:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::879381281606:user/alice-oliveira"
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:TagResource",
        "kms:UntagResource",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion",
        "kms:RotateKeyOnDemand"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::879381281606:user/alice-oliveira"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::879381281606:user/alice-oliveira"
      },
      "Action": ["kms:CreateGrant", "kms:ListGrants", "kms:RevokeGrant"],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "kms:GrantIsForAWSResource": "true"
        }
      }
    }
  ]
}
```

### AWS DEKs (Data Encryption Keys)

Data Encryption Keys (DEKs) are temporary keys used to encrypt data that exceeds the 4 KB limit of KMS keys.

- How They Work
  - KMS generates a DEK, which includes two parts:
    - **Plaintext Key**: Used for actual data encryption.
    - **Encrypted Key**: Safely stored and used later to re-encrypt the plaintext key.
  - The plaintext DEK encrypts your data.
  - The plaintext DEK is discarded immediately after use.
  - The encrypted DEK is stored alongside the encrypted data, enabling future decryption.

This ensures secure management of large datasets without exposing the plaintext DEK for longer than necessary.

## AWS KMS vs. AWS CloudHSM

AWS Key Management Service (KMS) and AWS CloudHSM are both services offered by AWS for key management and cryptographic operations, but they serve different purposes and offer different levels of control, flexibility, and security.

Key Differences between AWS KMS and AWS CloudHSM:

| **Aspect**                             | **AWS CloudHSM**                                                                                                                                                              | **AWS KMS**                                                                                                                                                             |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Purpose**                            | Dedicated hardware security module (HSM) in the cloud that provides granular control over cryptographic operations.                                                           | Fully managed key management service for encryption keys, designed for easy use with integrated AWS services.                                                           |
| **Use Cases**                          | Advanced use cases requiring full control over cryptographic modules, such as custom encryption, digital signing, or legacy systems. Ideal for FIPS 140-2 Level 3 compliance. | General-purpose key management, automatic key rotation, and integration with AWS services like S3, RDS, and EBS.                                                        |
| **Tenancy**                            | Single-Tenant                                                                                                                                                                 | Multi-Tenant                                                                                                                                                            |
| **High Availability: How to achieve?** | Create multiple HSMs (manually) over different AZs                                                                                                                            | Managed (automatically) by AWS                                                                                                                                          |
| **Scaling/Performance Responsibility** | Scaling requires manual intervention to add and configure additional HSM devices.                                                                                             | Highly scalable with automatic handling of higher workloads.                                                                                                            |
| **Key Management**                     | Full control over the keys, including HSM devices and cryptographic operations. Key rotation and lifecycle must be managed manually.                                          | AWS manages key generation, storage, rotation, and management, while you control key policies and lifecycle. Supports automatic key rotation for customer-managed keys. |
| **Key Access: Who controls it?**       | You                                                                                                                                                                           | You + AWS                                                                                                                                                               |
| **Keys: How to use?**                  | Customer code + Safenet APIs                                                                                                                                                  | AWS Management Console                                                                                                                                                  |
| **Keys: Where to use?**                | AWS & Your Network (VPN)                                                                                                                                                      | AWS                                                                                                                                                                     |
| **AWS Services Integration**           | Requires more custom integration. Used in applications requiring direct access to HSMs and custom cryptographic operations.                                                   | Seamless integration with a wide range of AWS services like S3, RDS, Lambda, and EC2 for centralized encryption and key management.                                     |
| **Compliance Standards**               | FIPS 140-2 Level 3 compliant, offering high physical security and control.                                                                                                    | FIPS 140-2 Level 2 compliant, providing a balance of security and convenience.                                                                                          |
| **Access & Authentication Policy**     | Quorum-based K of N                                                                                                                                                           | AWS IAM Policy                                                                                                                                                          |
| **Security**                           | Provides the highest level of security for cryptographic operations with dedicated HSM hardware and full user control.                                                        | High security with encryption and key access policies managed via AWS IAM, suitable for most general-purpose use cases.                                                 |
| **Performance**                        | Suitable for high-throughput cryptographic operations but scaling requires effort.                                                                                            | Scalable for high-throughput use cases like encrypting large volumes of data.                                                                                           |
| **Ease of Use**                        | Requires specialized knowledge for setup and management. Complexity is higher due to full user control over hardware and keys.                                                | Simplified key management, ideal for users who want to manage encryption keys without dealing with the underlying hardware.                                             |
| **Physical Control**                   | Provides exclusive, single-tenant access to physical HSM devices, offering higher isolation and control.                                                                      | As a managed, multi-tenant service, physical control is handled by AWS, reducing user control over hardware.                                                            |
| **Cost**                               | Higher cost due to dedicated hardware.                                                                                                                                        | Lower cost compared to CloudHSM due to the shared environment.                                                                                                          |
