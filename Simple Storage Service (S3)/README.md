## S3 Bucket Policies

S3 Bucket Policies are resource-based policies that define access permissions for S3 buckets and the objects within them. Unlike IAM identity policies, which attach to users, groups, or roles, bucket policies are directly associated with the bucket itself, allowing you to manage access from the resource's perspective.

Key features of Bucket Policies:

- Bucket policies define who can access the bucket and what actions they can perform. They apply to the bucket itself, objects within it, and allow for fine-grained access control.

- Allow or deny access to specific AWS accounts, including accounts outside your own organization.

- Bucket policies can permit or restrict access for unauthenticated users, enabling public or private configurations.

- You can specify actions (e.g., s3:GetObject) and resources (specific buckets or objects) to tightly control access.

- Use "Effect": "Allow" to grant permissions or "Effect": "Deny" to explicitly block access, even if other rules permit it.

### Example of a Bucket Policy

The following policy grants public read access to all objects in the bucket named secretcatproject:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "PublicRead",
    "Effect": "Allow",
    "Principal": "*",
    "Action": ["s3:GetObject"],
    "Resource": ["arn:aws:s3:::secretcatproject/*"]
  }
}
```

Explanation of Key Parameters:

- **version**: Defines the policy language version. "2012-10-17" is the latest and recommended version for all policies.

- **statement**: The core of the policy that defines permissions. A policy can contain multiple statements.

  - **sid**: A unique identifier for the statement. Here, "PublicRead" is used to label the rule for clarity.

  - **effect**: Determines whether the statement allows or denies the specified actions. "Allow" permits access in this example.

  - **principal**: Specifies who the permissions apply to. "\*" makes the bucket accessible to everyone, including anonymous users.

  - **action**: Defines the allowed actions. "s3:GetObject" permits downloading objects from the bucket.

  - **resource**: Identifies the specific bucket or objects the policy applies to.

## S3 Access Control List (ACLs)

S3 Access Control Lists (ACLs) are a legacy feature of Amazon S3 used to manage access permissions for buckets and objects.

Key features of ACLs:

- ACLs can be applied at both the bucket level (controlling access to the entire bucket) and the object level (controlling access to individual files within a bucket).

- ACLs are considered subresources of S3 buckets and objects, meaning they are tied to and managed as part of the bucket or object.

- While ACLs are still supported, they are considered an older method for access control in S3. Modern implementations recommend using bucket policies or IAM policies for more flexibility and granularity.

- ACLs provide a limited set of predefined permissions:

  - **READ**: Allows grantees to list the bucket contents or read the object data.

  - **WRITE**: Allows grantees to upload objects to the bucket or modify objects in it.

  - **READ_ACP**: Grants permission to read the Access Control List of the bucket or object.

  - **WRITE_ACP**: Grants permission to modify the Access Control List of the bucket or object.

  - **FULL_CONTROL**: Grants all the above permissions.

## S3 Static Hosting

Amazon S3 (Simple Storage Service) allows you to host static websites directly from an S3 bucket. This feature is designed for serving static assets such as HTML, CSS, JavaScript, images, and other files.

Key features of S3 Static Hosting:

- By default, S3 is accessed using AWS APIs for object storage and retrieval.

- When S3 Static Website Hosting is enabled, the bucket contents can be accessed directly over HTTP through a website endpoint.

- When hosting is enabled, AWS provides a public HTTP endpoint to access your website.

- To serve your website on a custom domain, the S3 bucket name must exactly match the domain name (e.g., example.com). Use AWS Route 53 (or another DNS service) to configure the domain's DNS settings. This typically involves setting up an Alias Record pointing to the S3 website endpoint.

## Object Versioning

Object versioning allows you to store multiple versions of the same object within a bucket. Any operation that modifies an object (such as overwrites or deletions) generates a new version instead of replacing or removing the original. Versioning is disabled by default, once enabled, it cannot be disabled, only suspended.

Key features of Object Versioning:

- When versioning is enabled, every version of an object gets a unique ID. When versioning is disabled, object IDs default to null.

- Objects are not fully deleted when versioning is enabled. Instead, they receive a delete marker and become hidden but recoverable. To permanently delete an object, all its versions must be deleted.

- Storage space is consumed by all object versions, and you are billed for all versions stored in the bucket.

### MFA Delete

MFA Delete enhances the security of S3 buckets with versioning by requiring multi-factor authentication (MFA) for sensitive operations. When MFA Delete is enabled, any changes to the bucket’s versioning state, such as enabling or suspending versioning, require MFA authentication. This ensures that only authorized users with access to the MFA device can make these changes.

Additionally, MFA is required to permanently delete object versions. To delete a version via the API, the request must include the serial number of the MFA device and a valid code generated by it. This feature provides an added layer of protection against accidental or unauthorized deletions, helping to safeguard critical data.

## Performance Optimization

### Single Data Stream vs. Multipart Upload

#### Single Data Stream

- A single data stream upload fails if interrupted, requiring a complete restart.

- Speed and reliability are limited to the capabilities of a single stream.

- Suitable for uploads up to 5GB in size.

#### Multipart Upload

- Data is divided into smaller parts for upload.

- Recommended for files larger than 100MB.

- Supports up to 10,000 parts, with each part ranging from 5MB to 5GB.

- Failed parts can be re-uploaded without restarting the entire process.

- Overall transfer speed is the combined speed of all parts being uploaded simultaneously.

### Accelerated Transfer

Accelerated Transfer uses Amazon S3 Transfer Acceleration to enable faster data transfers over long distances. This feature leverages Amazon CloudFront’s globally distributed edge locations to reduce latency and optimize transfer speed by routing data through the fastest network paths. It is particularly beneficial for time-sensitive or high-volume uploads. To use this feature, Transfer Acceleration must be enabled for the S3 bucket.

## S3 Object Encryption

S3 buckets themselves aren’t encrypted, but the objects stored within them are always encrypted. Encryption is applied at the object level, ensuring that each individual object within the bucket is encrypted based on the chosen encryption method. There are two types of encryption available: Client-Side Encryption (CSE), where the data is encrypted before it is uploaded to S3, and Server-Side Encryption (SSE), where the encryption and decryption processes are handled by S3 once the data is stored.

### Server-Side Encryption (SSE)

Mandatory encryption: Objects stored in S3 must be encrypted. You can choose which process to use for encryption. The default encryption method is SSE-S3. There are three types of SSE: SSE-C, SSE-S3 and SSE-KMS,

#### SSE-C (Customer-Provided Keys)

- **How it works**: You provide a key each time you upload an object to S3. This key is used for encryption and is destroyed by AWS after the encryption process.
- **When to use it**: Use SSE-C when you need to manage the encryption key yourself.
- **Important**: You must provide the same key again to decrypt the object later, as AWS does not store the key.

#### SSE-S3 (Amazon S3-Managed Keys)

- **How it works**: This is the default encryption method, where S3 manages the encryption and decryption processes using AES-256 encryption.
- **Key management**: S3 handles both the encryption keys and the entire process, making it the simplest option for users.

#### SSE-KMS (AWS Key Management Service)

- **How it works**: S3 uses AWS KMS to manage the encryption keys. KMS handles both encryption and decryption.
- **Key management**: You can use an AWS-managed KMS key, or you can bring your own customer-managed KMS key, which gives you more control over key management and permissions.
- **When to use it**: Use SSE-KMS when you need more control over key access and want to enforce stricter security policies.

### Client-Side Encryption (CSE)

- **How it works**: The object is encrypted before being uploaded to S3, and only the client with the decryption key can access the data in its original form.

- **Management**: involves encrypting the object before it is uploaded to S3. This means the encryption and decryption process happens on the client side, not in the cloud. This method offers more control over the encryption process, but it also requires more management from the user.

## S3 Bucket Keys

S3 Bucket Keys are a cost-saving feature in Amazon S3 that reduces the number of requests to AWS Key Management Service (KMS) when encrypting or decrypting objects in a bucket. It works by reusing a bucket-level key derived from your AWS KMS key instead of generating a unique key for each object.

### The Problem

When using SSE-KMS (Server-Side Encryption with AWS KMS), each object in an S3 bucket is encrypted with a unique data key generated by KMS. This setup can result in a large number of API requests to KMS, especially in buckets with high-frequency object uploads and downloads.

- **High Costs**: KMS API calls incur additional costs that can become significant at scale.
- **Performance Overhead**: Frequent KMS interactions can add latency to encryption and decryption processes.

### The Solution

S3 Bucket Keys address these challenges by introducing a bucket-level key. Here's how it works:

- A single bucket-level key is derived from your KMS key.
- This bucket key is used to generate data keys for encrypting and decrypting multiple objects within the bucket.
- The number of direct KMS API calls is reduced, resulting in lower costs and improved performance.

#### Benefits

- **Cost Savings**: Fewer KMS requests reduce API charges significantly for high-volume workloads.
- **Improved Performance**: Reduced latency in encryption and decryption.
- **Seamless Integration**: Can be enabled without major changes to your existing workflows.

By enabling S3 Bucket Keys, you maintain strong encryption while optimizing both cost and efficiency.
