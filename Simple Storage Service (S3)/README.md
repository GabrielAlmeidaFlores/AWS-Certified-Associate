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

## S3 Storage-Classes

### S3 Standard

- **Durability**: 99.999999999% (11 nines) for 10,000 objects—equivalent to losing one object every 10,000 years.

- **Availability**: Replicated across 3+ Availability Zones (AZs) within the region to ensure high fault tolerance.

- **Performance**: Millisecond first-byte latency for fast data access. Supports virtually unlimited storage with seamless scaling for high-performance applications.

- **Cost**:

  - **Storage**: Charged per GB/month.
  - **Transfer**:
    - **Inbound**: Free.
    - **Outbound**: Charged per GB.
  - **Requests**: Charged per 1,000 PUT, GET, and other requests.
  - **Flexibility**: No retrieval fees, no minimum duration, or object size constraints.

- **Use Case**: Frequently accessed, critical, and irreplaceable data (e.g., real-time analytics, websites, mobile applications).

### S3 Standard-IA

- **Durability**: Same as S3 Standard.

- **Availability**: Data is replicated across 3+ AZs for resilience.

- **Performance**:

  - Millisecond first-byte latency ensures fast access when needed.
  - Designed for less frequent access compared to S3 Standard.

- **Costs**:

  - Lower storage cost than S3 Standard but includes a retrieval fee per GB.

  - **Minimum charges**:

    - **Duration**: 30 days (billed even if deleted earlier).

    - **Object size**: 128 KB (smaller objects are charged as 128 KB).

- **Use Case**: Long-lived data that is important but accessed less frequently (e.g., backups, disaster recovery).

### S3 One Zone-IA

- **Durability**: Same as Standard-IA, but data is stored in a single AZ, increasing risk in case of an AZ outage.

- **Performance**: Millisecond first-byte latency.

- **Costs**:

  - Lower storage costs than Standard-IA.

  - Retrieval fees, 30-day minimum duration, and 128 KB minimum object size apply.

- **Use Case**: Long-lived data that is non-critical and easily replaceable. Suitable for scenarios where cost is prioritized over resilience (e.g., secondary backups, non-essential logs).

### S3 Glacier Instant Retrieval

- **Durability**: Same as S3 Standard.

- **Availability**: Replicated across 3+ AZs.

- **Performance**: Millisecond first-byte latency, designed for archival data with infrequent but immediate access needs.

- **Costs**:

  - Lowest storage cost for immediate retrieval class.

  - **Minimum charges**:

    - **Duration**: 90 days.

    - **Object size**: 128 KB.

  - Retrieval incurs a per GB fee.

- **Use Case**: Archive data that is accessed occasionally (e.g., quarterly reports, compliance data).

### S3 Glacier Flexible Retrieval

- **Durability**: Same as S3 Standard.

- **Availability**: Replicated across 3+ AZs.

- **Performance**: Retrieval requires time:

  - **Expedited**: 1–5 minutes (higher cost).

  - **Standard**: 3–5 hours (cost-effective).

  - **Bulk**: 5–12 hours (lowest cost).

- **Costs**:

  - Lower storage cost than Glacier Instant.

  - **Minimum charges**:

    - **Duration**: 90 days.

    - **Object size**: 40 KB.

  - Retrieval incurs additional costs.

- **Use Case**: Archival data where access is infrequent, and retrieval latency of minutes to hours is acceptable (e.g., historical records, legal archives).

### S3 Glacier Deep Archive

- **Durability**: Same as S3 Standard.

- **Availability**: Replicated across 3+ AZs.

- **Performance**: Retrieval requires hours to days:

  - **Standard**: 12 hours.

  - **Bulk**: Up to 48 hours.

- **Costs**:

  - Lowest storage cost across all S3 classes.

  - **Minimum charges**:

    - **Duration**: 180 days.

    - **Object size**: 40 KB.

- **Use Case**: Rarely accessed, long-term archival data where retrieval time in hours or days is acceptable (e.g., compliance records, tax documents).

### S3 Intelligent-Tiering

- **Key Features**: Automatically transitions objects between frequent and infrequent access tiers based on usage patterns. Supports additional archival tiers (Glacier and Glacier Deep Archive) for further cost optimization.

- **Durability**: Same as S3 Standard.

- **Availability**: Replicated across 3+ AZs.

- **Costs**:

  - Monitoring and automation fee per object.

  - No retrieval fees or minimum duration/object size.

- **Use Case**: Data with unpredictable or changing access patterns, such as machine learning datasets, media libraries, or user-generated content.

## S3 Lifecycle

S3 Lifecycle helps you store objects cost effectively throughout their lifecycle by transitioning them to lower-cost storage classes, or, deleting expired objects on your behalf. To manage the lifecycle of your objects, create an S3 Lifecycle configuration for your bucket. An S3 Lifecycle configuration is a set of rules that define actions that Amazon S3 applies to a group of objects. There are two types of actions:

- **Transition actions**: These actions define when objects transition to another storage class. For example, you might choose to transition objects to the S3 Standard-IA storage class 30 days after creating them, or archive objects to the S3 Glacier Flexible Retrieval storage class one year after creating them. For more information, see Understanding and managing Amazon S3 storage classes. There are costs associated with lifecycle transition requests.

- **Expiration actions**: These actions define when objects expire. Amazon S3 deletes expired objects on your behalf. For example, you might to choose to expire objects after they have been stored for a regulatory compliance period. For more information, see Expiring objects. There are potential costs associated with lifecycle expiration only when you expire objects in a storage class with a minimum storage duration.

### Existing and New Objects

When you add a Lifecycle configuration to a bucket, the configuration rules apply to both existing objects and objects that you add later. For example, if you add a Lifecycle configuration rule today with an expiration action that causes objects to expire 30 days after creation, Amazon S3 will queue for removal any existing objects that are more than 30 days old.

## S3 Replication

You can use replication to enable automatic, asynchronous copying of objects across Amazon S3 buckets. Buckets that are configured for object replication can be owned by the same AWS account or by different accounts. You can replicate objects to a single destination bucket or to multiple destination buckets. The destination buckets can be in different AWS Regions or within the same Region as the source bucket.

### Types of Replication

- **Live replication**: To automatically replicate new and updated objects as they are written to the source bucket, use live replication. Live replication doesn't replicate any objects that existed in the bucket before you set up replication. To replicate objects that existed before you set up replication, use on-demand replication.

- **On-demand replication**: To replicate existing objects from the source bucket to one or more destination buckets on demand, use S3 Batch Replication. Batch Replication replicates existing objects to different buckets as an on-demand option. Unlike live replication, these jobs can be run as needed.

### Forms of Replication

- **Cross-Region Replication (CRR)**: Facilitates the replication of objects between buckets located in different AWS Regions. Buckets can belong to the same AWS account or different AWS accounts.

- **Same-Region Replication (SRR)**: Enables replication of objects between buckets in the same AWS Region. Buckets can be in the same AWS account or across different AWS accounts.

### Key Features and Options

- **Replication Scope**: You can replicate all objects in a bucket or define replication rules to replicate a subset of objects based on object prefixes or tags.

- **Storage Class**: By default, the replicated objects retain the same storage class as the source object. You can configure replication to use a different storage class (e.g., Standard-IA or One Zone-IA) to optimize costs for the replicated data.

- **Object Ownership**: Replicated objects are owned by the source account by default. When replicating objects to a bucket in a different AWS account, ensure that the destination account owns the objects. Misconfigured ownership can result in access issues where the destination account cannot access or read the replicated objects. To avoid this, configure Bucket Ownership Controls or enable the BucketOwnerFullControl ACL during replication.

- **Replication Time Control (RTC)**: RTC provides predictable replication performance with a service level agreement (SLA) of replicating 99.99% of objects within 15 minutes. RTC is beneficial for use cases requiring low-latency replication, such as applications with stringent RTO (Recovery Time Objective) requirements.

- **Non-Retroactive Replication**: Replication applies only to objects created or updated after replication is enabled. Existing objects present in the source bucket before replication setup are not replicated automatically.

- **Versioning Requirement**: Both the source and destination buckets must have versioning enabled for replication to function. Versioning ensures that object versions and delete markers are handled correctly during replication.

- **Batch Replication**: To replicate objects that existed before replication was enabled, use Batch Replication, which is a separate feature from regular replication. Batch Replication enables one-time replication of objects created before the replication configuration was applied. It can also be used to re-replicate objects in cases of replication failures.

- **One-Way Replication**: Replication is a unidirectional process, meaning data flows from the source bucket to the destination bucket. Reverse replication must be explicitly configured if needed.

### Limitations and Exclusions

- **Unsupported Storage Classes**: Objects stored in Glacier or Glacier Deep Archive storage classes are not replicated. To replicate these objects, transition them to a supported storage class before replication.

- **Delete Operations**: By default, delete operations are not replicated to the destination bucket. If desired, you can enable Delete Marker Replication to replicate delete markers. Delete marker replication is particularly useful when synchronizing lifecycle policies between source and destination buckets.

## S3 Select and Glacier Select

### S3 Select

S3 Select is a feature of Amazon S3 that allows you to query and retrieve a subset of data from an S3 object using standard SQL expressions. This reduces the amount of data transferred and speeds up data analysis by working directly within the object.

S3 Select Key Features:

- **Query Efficiency**: Supports querying structured data formats such as CSV, JSON, and Parquet stored in S3. Reduces network bandwidth by retrieving only the required data instead of the entire object.

- **Standard SQL Support**: Use SQL expressions to filter, project, and aggregate data.
  Examples include selecting specific columns, filtering rows based on conditions, and performing basic aggregations.

- **Performance Benefits**: Speeds up data analysis by processing the data server-side within S3 before transferring it to the client. Particularly beneficial for large datasets.

### Glacier Select

Glacier Select enables you to perform SQL-based queries directly on data stored in Amazon S3 Glacier. This allows you to retrieve only the required data without restoring the entire archive object, optimizing retrieval costs and efficiency.

Glacier Select Key Features:

- **SQL-Based Queries**: Supports querying archived data using standard SQL expressions, similar to S3 Select. Query formats include CSV and JSON.

- **Query Tiers**:

  - Retrievals can be performed using three access tiers to balance speed and cost:
    - **Expedited**: Fastest, suitable for small objects or urgent queries.
    - **Standard**: Cost-effective with moderate retrieval times.
    - **Bulk**: Lowest cost, best for non-urgent, large-scale queries.

- **Cost Efficiency**: Minimizes the cost of querying archived data by retrieving only the required subset instead of restoring entire archives.

- **Use Cases**:
  - Searching archived logs for compliance audits.
  - Extracting specific fields from historical data for analysis.
  - Cost-effective querying of infrequently accessed data.

## S3 Events Notifications

S3 Event Notifications allow you to automatically trigger actions or workflows in response to specific events in your S3 bucket, such as object uploads, deletions, or updates. These notifications integrate seamlessly with various AWS services to enable event-driven architectures.

Currently, Amazon S3 can publish notifications for the following events:

- New object created events
- Object removal events
- Restore object events
- Reduced Redundancy Storage (RRS) object lost events
- Replication events
- S3 Lifecycle expiration events
- S3 Lifecycle transition events
- S3 Intelligent-Tiering automatic archival events
- Object tagging events
- Object ACL PUT events

### Destinations for Notifications

- **Amazon Simple Notification Service (SNS)**: Used for sending notifications to subscribers via email, SMS, or HTTP endpoints. Ideal for broadcasting events to multiple endpoints.

- **Amazon Simple Queue Service (SQS)**: Sends event messages to an SQS queue for asynchronous processing. Suitable for decoupled workflows with reliable message delivery.

- **AWS Lambda**: Invokes a Lambda function for serverless processing of the event. Commonly used for data processing, triggering workflows, or running custom logic.

### Configuration

- **Permissions**: The bucket must have a properly configured bucket policy granting permissions to the destination service (e.g., Lambda, SQS, SNS).

- **Notification Rules**: Each S3 bucket can have multiple notification configurations, but each must be uniquely defined (e.g., by event type or destination).

- **Region-Specific Behavior**: Notifications work within the same AWS Region. If the destination is cross-region, additional setup is required.

## Requester Pays

In general, bucket owners pay for all Amazon S3 storage and data transfer costs that are associated with their bucket. However, you can configure a bucket to be a Requester Pays bucket. With Requester Pays buckets, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket. The bucket owner always pays the cost of storing data.

Typically, you configure buckets to be Requester Pays buckets when you want to share data but not incur charges associated with others accessing the data. For example, you might use Requester Pays buckets when making available large datasets, such as zip code directories, reference data, geospatial information, or web crawling data.

> [!IMPORTANT]
> If you enable Requester Pays on a bucket, anonymous access to that bucket is not allowed.

After you configure a bucket to be a Requester Pays bucket, requesters must show they understand that they will be charged for the request and for the data download. To show they accept the charges, requesters must either include x-amz-request-payer as a header in their API request for DELETE, GET, HEAD, POST, and PUT requests, or add the RequestPayer parameter in their REST request. For CLI requests, requesters can use the --request-payer parameter.

Example – Using Requester Pays when deleting an object:

```curl

DELETE /Key+?versionId=VersionId HTTP/1.1
Host: Bucket.s3.amazonaws.com
x-amz-mfa: MFA
x-amz-request-payer: RequestPayer
x-amz-bypass-governance-retention: BypassGovernanceRetention
x-amz-expected-bucket-owner: ExpectedBucketOwner
```
