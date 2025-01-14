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

### Purpose & Use Cases

#### AWS KMS

- Fully managed key management service for encryption keys.
- Designed for use cases where you need to manage and use encryption keys easily with integrated AWS services.
- Supports symmetric and asymmetric keys.
- Ideal for general-purpose key management, automatic key rotation, and integration with various AWS services (e.g., S3, RDS, EBS).

#### AWS CloudHSM

- Dedicated hardware security module (HSM) in the cloud that provides more granular control over cryptographic operations.
- Provides a physical device (HSM) for secure key storage and cryptographic operations.
- You fully manage the HSMs, including the keys and the cryptographic operations.
- Advanced use cases where you need full control over the cryptographic module, such as custom encryption, digital signing, or if you're using legacy systems that require HSMs. Ideal for applications requiring FIPS 140-2 Level 3 compliance.

### Key Management and Control

#### AWS KMS

- AWS takes care of key generation, storage, rotation, and management.
- Key management is easier because it integrates directly with AWS services.
- While you can manage key policies and lifecycle, you don't control the underlying hardware where the keys are stored.
- Supports automatic key rotation for customer-managed keys.

#### AWS CloudHSM

- You have full control over the keys, including the HSM device itself and the cryptographic operations.
- You can generate, store, and manage your own keys within the HSM.
- You must manually rotate keys and manage their lifecycle.
- You can perform operations that require fine-tuned control over cryptographic algorithms or custom implementations.

### Security and Compliance

#### AWS KMS

- FIPS 140-2 Level 2 compliant for the underlying hardware.
- Offers high security but not at the same level of physical separation or control as CloudHSM.
- Encryption and key access policies are managed using IAM (Identity and Access Management).
- Ideal for most general-purpose use cases and integrated with AWS services for compliance (e.g., GDPR, HIPAA).

#### AWS CloudHSM

- FIPS 140-2 Level 3 compliant (higher level of physical security and control than KMS).
- Offers the highest level of security for cryptographic operations because of dedicated HSM hardware.
- You control the HSM and the keys it stores, which can be essential for high-security environments or compliance with stricter regulations.

### Performance and Scalability

#### AWS KMS

- Scalable and supports high-throughput use cases like encrypting large volumes of data (up to 4 KB per operation for KMS keys, or using DEKs for larger data).
- Managed by AWS, so scaling is automatic as your usage grows.

#### AWS CloudHSM

- Scalable, but more complex: While you can scale CloudHSM, it requires manual intervention to add more HSM devices, and you're responsible for configuring them.
- Suitable for workloads where you need to handle high-throughput cryptographic operations, but scaling requires more effort compared to KMS.

### Integration with AWS Services

#### AWS KMS

- Seamless integration with a wide range of AWS services like Amazon S3, RDS, Lambda, and EC2.
- Supports encryption and key management in a centralized, easy-to-use way for AWS services.

#### AWS CloudHSM

- Requires more custom integration.
- Used in applications that require direct access to HSMs and custom cryptographic operations.
- Not as tightly integrated with AWS services as KMS is.
