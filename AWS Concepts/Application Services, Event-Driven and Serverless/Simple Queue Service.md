# Simple Queue Service

Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables decoupling and scaling of microservices, distributed systems, and serverless applications. SQS eliminates the complexity and overhead associated with managing and operating message-oriented middleware, empowering developers to focus on differentiating work. SQS offers two types of message queues: Standard and FIFO (First-In-First-Out). These queues are public, highly available, and designed to handle high throughput, making them suitable for a wide range of use cases, from task scheduling to event-driven architectures.

SQS is billed based on the number of requests made to the service. A single request can include between 1 and 10 messages, with a total payload size of up to 64 KB. Individual messages can be up to 256 KB in size. For larger payloads, SQS integrates with Amazon S3 through the SQS Extended Client Library, which allows messages to reference larger objects stored in S3. This documentation will delve into the key features, configurations, and best practices for using SQS effectively.

## Key Features of Amazon SQS

### Public, Fully Managed, and Highly Available Queues

SQS is a public service, meaning it is accessible over the internet and can be integrated into applications hosted anywhere. It is fully managed by AWS, ensuring that users do not need to worry about infrastructure provisioning, maintenance, or scaling. SQS queues are highly available, with messages redundantly stored across multiple availability zones within a region. This design ensures that messages are durable and accessible even in the event of hardware failures or other disruptions.

### Visibility Timeout

When a message is polled from an SQS queue, it becomes invisible to other consumers for a configurable period known as the VisibilityTimeout. This mechanism prevents multiple consumers from processing the same message simultaneously. The default VisibilityTimeout is 30 seconds, but it can be adjusted to a maximum of 12 hours. If the message is not deleted within this timeout period, it reappears in the queue, allowing for reprocessing. This feature is particularly useful in scenarios where message processing might fail due to transient issues, such as network latency or temporary resource unavailability.

VisibilityTimeout can be set at the queue level or overridden at the message level. For example, a queue might have a default timeout of 5 minutes, but a specific message could be configured to remain invisible for 10 minutes. This flexibility allows developers to fine-tune message processing behavior based on the requirements of their application.

### Polling Mechanisms: Short Polling vs. Long Polling

Amazon SQS (Simple Queue Service) supports two types of polling mechanisms for retrieving messages:

- **Short Polling**: Returns a response immediately, even if the queue is empty. This approach is ideal for applications that require low latency. However, it can lead to higher costs and inefficiency due to the increased number of empty responses, especially in queues with low message frequency.

- **Long Polling**: Waits for a specified period (up to 20 seconds) for messages to become available in the queue before returning a response. This approach significantly reduces the number of empty responses, lowers costs, and improves overall efficiency. Long polling is particularly beneficial for applications that prioritize cost savings and can tolerate slightly higher latency.

### Encryption at Rest and In Transit

SQS ensures the security of messages through two types of encryption:

- **Encryption at Rest**: Messages can be encrypted at rest using AWS Key Management Service (KMS). This allows you to create and manage encryption keys, ensuring that data stored in SQS queues is protected from unauthorized access, even if the underlying storage is compromised. You can also configure customer-managed keys (CMKs) for greater control over key policies and permissions.

- **In-Transit Encryption**: SQS supports in-transit encryption using HTTPS, which secures data as it travels between the client and the SQS service. This prevents eavesdropping or tampering during transmission. Additionally, SQS integrates with AWS Identity and Access Management (IAM) to enforce fine-grained access control, ensuring that only authorized users, applications, or services can send, receive, or manage messages.

### Dead-Letter Queues

Dead-letter queues (DLQs) are a powerful feature for handling messages that cannot be processed successfully. A DLQ is a separate SQS queue that receives messages from a source queue after they have exceeded the maximum number of receive attempts (maxReceiveCount). This prevents problematic messages from clogging the main queue and allows developers to analyze and debug failed messages.

A redrive policy defines the relationship between the source queue and the DLQ, specifying the conditions under which messages are moved. For example, if a message is received more than five times without being deleted, it is automatically moved to the DLQ. The enqueue timestamp of the message remains unchanged, ensuring that the original timing information is preserved.

### Integration with Auto Scaling Groups (ASGs) and AWS Lambda

SQS integrates seamlessly with other AWS services, such as Auto Scaling Groups (ASGs) and AWS Lambda. ASGs can scale the number of EC2 instances based on the length of an SQS queue, ensuring that the application can handle varying workloads. Similarly, AWS Lambda functions can be triggered by new messages in an SQS queue, enabling serverless architectures that automatically process messages as they arrive.

### Access Control with Identity Policies and Queue Policies

Access to SQS queues can be controlled using identity policies or queue policies. Identity policies are attached to IAM users, groups, or roles and define the permissions they have for interacting with SQS queues. Queue policies, on the other hand, are attached directly to the queue and specify which AWS accounts or IAM entities are allowed to perform actions on the queue.

## SQS Standard vs. FIFO Queues

### SQS Standard Queues

SQS Standard queues are designed for high throughput and scalability. They offer best-effort ordering, meaning that messages are generally delivered in the order they are sent, but there is no strict guarantee. Standard queues also provide at-least-once delivery, which means that a message might be delivered more than once under certain conditions. This is acceptable for many use cases, such as task scheduling or event broadcasting, where occasional duplicates can be handled gracefully.

Standard queues are ideal for applications that require near-unlimited throughput and can tolerate some degree of message duplication or out-of-order delivery. They are also well-suited for decoupling components of distributed systems, enabling worker pools, and batch processing.

### SQS FIFO Queues

SQS FIFO queues are designed for applications that require strict message ordering and exactly-once processing. FIFO queues ensure that messages are delivered in the order they are sent and that no duplicates are introduced. This makes them suitable for use cases such as financial transactions, where the order and uniqueness of messages are critical.

FIFO queues have a throughput limit of 300 transactions per second (TPS) without batching and 3,000 TPS with batching. Batching allows multiple messages to be grouped into a single request, increasing throughput while maintaining order and uniqueness. FIFO queues must have a .fifo suffix in their name, which helps distinguish them from Standard queues and ensures compatibility with the FIFO processing logic.

## SQS Extended Client Library

The SQS Extended Client Library is a tool for handling messages that exceed the SQS size limit of 256 KB. It integrates SQS with Amazon S3, allowing larger payloads to be stored in S3 and referenced by smaller messages in the SQS queue. The library handles the integration workload, including uploading payloads to S3, storing the S3 object reference in the SQS message, and deleting the S3 object when the message is processed.

For example, when sending a message, the library uploads the payload to S3 and stores the S3 object key in the SQS message. When receiving a message, the library retrieves the payload from S3 using the object key. This approach enables the handling of large payloads while leveraging the scalability and durability of S3.

## SQS Delay Queues

SQS Delay Queues allow messages to be delayed for a specified period before they become available for processing. The delay is controlled by the DelaySeconds parameter, which can be set at the queue level or overridden at the message level. The minimum delay is 0 seconds, and the maximum is 15 minutes.

If a message specifies a delay, it overrides the queue-level delay. This flexibility allows developers to fine-tune message delivery timing based on specific requirements. Note that message-level delays are not supported for FIFO queues, as they could disrupt the strict ordering of messages.

## Summary Table of Key Features

| Feature               | Description                                                               |
| --------------------- | ------------------------------------------------------------------------- |
| VisibilityTimeout     | Configurable period during which polled messages are invisible.           |
| Polling Mechanisms    | Short polling (immediate response) vs. long polling (waits for messages). |
| Encryption            | Supports encryption at rest (KMS) and in transit (HTTPS).                 |
| Dead-Letter Queues    | Handles problematic messages by moving them to a separate queue.          |
| Integration with ASGs | Auto Scaling Groups can scale based on queue length.                      |
| Access Control        | Managed via identity policies or queue policies.                          |
| Standard Queues       | High throughput, best-effort ordering, at-least-once delivery.            |
| FIFO Queues           | Strict ordering, exactly-once processing, limited throughput.             |
| SQS Extended Library  | Enables handling of messages larger than 256 KB using S3.                 |
| Delay Queues          | Delays message visibility for up to 15 minutes.                           |
