## CoudTrail

CloudTrail is an AWS service that enables operational and risk auditing, governance, and compliance for your AWS account. It provides visibility into user and service actions by logging API calls and activities as CloudTrail events. These events include actions taken through the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.

When activity occurs in your AWS account, it is automatically recorded as a CloudTrail event. By tracking these actions, CloudTrail helps ensure compliance, supports operational troubleshooting, and enhances security monitoring.

Key Features of AWS CloudTrail:

- **Logs API Calls/Activities**: CloudTrail records actions taken by users, roles, or AWS services as events.

- **Enabled by Default**: CloudTrail is enabled automatically for all AWS accounts, providing the 90-day Event History. To customize the service beyond this, you need to create one or more Trails.

- **Event History**: By default, CloudTrail provides 90 days of event history, accessible through the Event History console. This history is stored at no additional cost.

- **Events Categories**: CloudTrail offers two categories of events:

  - **Management Events**: Management Events are logged by default. These are actions performed on AWS resources (e.g., creating an instance, deleting a bucket).

  - **Data Events**: Data Events are not logged by default and require explicit configuration. These are actions performed on the data within resources (e.g., reading or writing data to an S3 bucket).

- **Regional and Global Configurations**: CloudTrail is a regional service but can be configured to record events across all regions.

- **Trails for Customization**: Trails allow you to customize CloudTrail by defining how and where events are logged, without a trail, logs are only stored for 90 days. By default, trails store only management events. Trails can save logs to:

  - Amazon S3 buckets for long-term storage.
  - CloudWatch Logs for monitoring and real-time alerting.

- **CloudTrail is Not Real-Time**: There is a delay in event logging, so activities may not appear instantly in the logs.
