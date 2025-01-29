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

## CloudWatch

Amazon CloudWatch is basically a metrics repository. An AWS service—such as Amazon EC2—puts metrics into the repository, and you retrieve statistics based on those metrics. If you put your own custom metrics into the repository, you can retrieve statistics on these metrics as well.

You can use metrics to calculate statistics and then present the data graphically in the CloudWatch console.

You can configure alarm actions to stop, start, or terminate an Amazon EC2 instance when certain criteria are met. In addition, you can create alarms that initiate Amazon EC2 Auto Scaling and Amazon Simple Notification Service (Amazon SNS) actions on your behalf.

### CloudWatch Concepts

#### Namespace

A namespace is a container for CloudWatch metrics. Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics. The AWS namespaces typically use the following naming convention: `AWS/service`. For example, Amazon EC2 uses the `AWS/EC2` namespace. For the list of AWS namespaces, see AWS services that publish CloudWatch metrics.

#### Datapoint

A CloudWatch datapoint is a single data element representing a measurement collected from a CloudWatch metric at a specific time. Datapoints are used to monitor and analyze the performance and health of AWS resources or custom metrics. A Datapoint includes the following components:

- **Timestamp**: The time the data point was recorded.

- **Value**: The metric value at the specified timestamp.

- **(Optional) Unit of Measure**: Describes the metric's unit, such as Percent or Bytes.

#### Metrics

Metrics are the fundamental concept in CloudWatch. A metric represents a time-ordered set of data points that are published to CloudWatch. Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time. For example, the CPU usage of a particular EC2 instance is one metric provided by Amazon EC2. The data points themselves can come from any application or business activity from which you collect data.

By default, many AWS services provide metrics at no charge for resources (such as Amazon EC2 instances, Amazon EBS volumes, and Amazon RDS DB instances). For a charge, you can also enable detailed monitoring for some resources, such as your Amazon EC2 instances, or publish your own application metrics. For custom metrics, you can add the data points in any order, and at any rate you choose. You can retrieve statistics about those data points as an ordered set of time-series data.

Metrics exist only in the Region in which they are created. Metrics cannot be deleted, but they automatically expire after 15 months if no new data is published to them. Data points older than 15 months expire on a rolling basis; as new data points come in, data older than 15 months is dropped.

Metrics are uniquely defined by a name, a namespace, and zero or more dimensions. Each data point in a metric has a time stamp, and (optionally) a unit of measure. You can retrieve statistics from CloudWatch for any metric.

#### Dimension

A dimension is a name/value pair that is part of the identity of a metric. You can assign up to 30 dimensions to a metric. Every metric has specific characteristics that describe it, and you can think of dimensions as categories for those characteristics. Dimensions help you design a structure for your statistics plan. Because dimensions are part of the unique identifier for a metric, whenever you add a unique name/value pair to one of your metrics, you are creating a new variation of that metric.

AWS services that send data to CloudWatch attach dimensions to each metric. You can use dimensions to filter the results that CloudWatch returns. For example, you can get statistics for a specific EC2 instance by specifying the InstanceId dimension when you search for metrics.

#### Resolution

Each metric is one of the following:

- **Standard Resolution**: Provides metrics at 60-second granularity (default for most AWS services).

- **High Resolution**: Provides metrics at 1-second granularity (useful for high-frequency monitoring).

Metrics produced by AWS services are standard resolution by default. When you publish a custom metric, you can define it as either standard resolution or high resolution. When you publish a high-resolution metric, CloudWatch stores it with a resolution of 1 second, and you can read and retrieve it with a period of 1 second, 5 seconds, 10 seconds, 30 seconds, or any multiple of 60 seconds.

High-resolution metrics can give you more immediate insight into your application's sub-minute activity. Keep in mind that every PutMetricData call for a custom metric is charged, so calling PutMetricData more often on a high-resolution metric can lead to higher charges.

If you set an alarm on a high-resolution metric, you can specify a high-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds. There is a higher charge for high-resolution alarms with a period of 10 or 30 seconds.

##### Metrics Retention

CloudWatch retains metric data as follows:

- Data points with a period of less than 60 seconds are available for 3 hours. These data points are high-resolution custom metrics.
- Data points with a period of 60 seconds (1 minute) are available for 15 days
- Data points with a period of 300 seconds (5 minutes) are available for 63 days
- Data points with a period of 3600 seconds (1 hour) are available for 455 days (15 months)

Data points that are initially published with a shorter period are aggregated together for long-term storage. For example, if you collect data using a period of 1 minute, the data remains available for 15 days with 1-minute resolution. After 15 days this data is still available, but is aggregated and is retrievable only with a resolution of 5 minutes. After 63 days, the data is further aggregated and is available with a resolution of 1 hour.

#### Percentile

A percentile indicates the relative standing of a value in a dataset. For example, the 95th percentile means that 95 percent of the data is lower than this value and 5 percent of the data is higher than this value. Percentiles help you get a better understanding of the distribution of your metric data.

Percentiles are often used to isolate anomalies. In a normal distribution, 95 percent of the data is within two standard deviations from the mean and 99.7 percent of the data is within three standard deviations from the mean. Any data that falls outside three standard deviations is often considered to be an anomaly because it differs so greatly from the average value. For example, suppose that you are monitoring the CPU utilization of your EC2 instances to ensure that your customers have a good experience. If you monitor the average, this can hide anomalies. If you monitor the maximum, a single anomaly can skew the results. Using percentiles, you can monitor the 95th percentile of CPU utilization to check for instances with an unusually heavy load.

Some CloudWatch metrics support percentiles as a statistic. For these metrics, you can monitor your system and applications using percentiles as you would when using the other CloudWatch statistics (Average, Minimum, Maximum, and Sum). For example, when you create an alarm, you can use percentiles as the statistical function. You can specify the percentile with up to ten decimal places (for example, p95.0123456789).

Percentile statistics are available for custom metrics as long as you publish the raw, unsummarized data points for your custom metric. Percentile statistics are not available for metrics when any of the metric values are negative numbers.

CloudWatch needs raw data points to calculate percentiles. If you publish data using a statistic set instead, you can only retrieve percentile statistics for this data if one of the following conditions is true:

The SampleCount value of the statistic set is 1 and Min, Max, and Sum are all equal.

The Min and Max are equal, and Sum is equal to Min multiplied by SampleCount.

The following AWS services include metrics that support percentile statistics.

- API Gateway
- Application Load Balancer
- Amazon EC2
- Elastic Load Balancing
- Kinesis
- Lambda
- Amazon RDS

### CloudWatch Alarms

ou can use an alarm to automatically initiate actions on your behalf. An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time. The action is a notification sent to an Amazon SNS topic or an Auto Scaling policy. You can also add alarms to dashboards.

Alarms invoke actions for sustained state changes only. CloudWatch alarms do not invoke actions simply because they are in a particular state. The state must have changed and been maintained for a specified number of periods.

When creating an alarm, select an alarm monitoring period that is greater than or equal to the metrics resolution. For example, basic monitoring for Amazon EC2 provides metrics for your instances every 5 minutes. When setting an alarm on a basic monitoring metric, select a period of at least 300 seconds (5 minutes). Detailed monitoring for Amazon EC2 provides metrics for your instances with a resolution of 1 minute. When setting an alarm on a detailed monitoring metric, select a period of at least 60 seconds (1 minute).

### CloudWatch Logs

Amazon CloudWatch Logs is a regional, public service that allows users to store, monitor, and access logging data from various sources. It is designed to provide a centralized logging solution for applications, systems, and services across AWS, on-premises environments, IoT devices, or any application capable of generating logs.

Key Concepts and Features:

- **Regional Service**: CloudWatch Logs operates within specific AWS regions, ensuring that log data remains within the designated region for compliance and performance reasons.

- **Public Service**: It is publicly accessible to store, monitor, and access log data from multiple environments, whether in the cloud or on-premises.

- **Log Ingestion**: CloudWatch Logs can ingest logs from:

  - AWS services (e.g., Lambda, EC2, RDS)

  - On-premises servers and applications using CloudWatch Agent or APIs

  - IoT devices or custom applications

- **CloudWatch Agent (CWAgent)**: The CloudWatch Agent enables system and custom application logging, allowing for seamless integration of application-specific logs with CloudWatch Logs.

- **Integration with AWS Services**: CloudWatch Logs integrates with various AWS services like Lambda, S3, Kinesis, and SNS for processing, storage, and alerting on log data.

- **Log Management Components**: CloudWatch Logs provides a structured approach to organizing and managing log data through its core components: Log Streams and Log Groups. These components work together to ensure logs are stored efficiently, categorized logically, and accessible for analysis and monitoring.

  - **Log Streams**: A log stream is a sequence of log events that share the same source. Each separate source of logs in CloudWatch Logs makes up a separate log stream.

  - **Log Groups**: A log group is a group of log streams that share the same retention, monitoring, and access control settings. You can define log groups and specify which streams to put into each group. There is no limit on the number of log streams that can belong to one log group.

- **Metric Filters**: Metric filters analyze log data in real time to extract key performance indicators or errors and convert them into metrics. These metrics can be used for monitoring and triggering alarms.

- **On-Premises and AWS Compatibility**: CloudWatch Logs can be used for log management scenarios across both AWS and on-premises environments, making it a versatile solution for hybrid setups.

- **Default for Log Management**: For any log management needs within AWS, CloudWatch Logs is the default choice due to its seamless integration with AWS services, scalability, and robust feature set.

## X-Ray

AWS X-Ray receives traces from your application, in addition to AWS services your application uses that are already integrated with X-Ray. Instrumenting your application involves sending trace data for incoming and outbound requests and other events within your application, along with metadata about each request. Many instrumentation scenarios require only configuration changes. For example, you can instrument all incoming HTTP requests and downstream calls to AWS services that your Java application makes.

AWS services that are integrated with X-Ray can add tracing headers to incoming requests, send trace data to X-Ray, or run the X-Ray daemon. For example, AWS Lambda can send trace data about requests to your Lambda functions, and run the X-Ray daemon on workers to make it simpler to use the X-Ray SDK.

Instead of sending trace data directly to X-Ray, each client SDK sends JSON segment documents to a daemon process listening for UDP traffic. The X-Ray daemon buffers segments in a queue and uploads them to X-Ray in batches. The daemon is available for Linux, Windows, and macOS, and is included on AWS Elastic Beanstalk and AWS Lambda platforms.

X-Ray uses trace data from the AWS resources that power your cloud applications to generate a detailed trace map. The trace map shows the client, your front-end service, and backend services that your front-end service calls to process requests and persist data. Use the trace map to identify bottlenecks, latency spikes, and other issues to solve to improve the performance of your applications.

## VPC Flow Logs

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to the following locations: Amazon CloudWatch Logs, Amazon S3, or Amazon Data Firehose. After you create a flow log, you can retrieve and view the flow log records in the log group, bucket, or delivery stream that you configured.

Flow log data is collected outside of the path of your network traffic, and therefore does not affect network throughput or latency. You can create or delete flow logs without any risk of impact to network performance.

VPC Flow Logs Key Concepts:

- **Purpose**: Capture metadata only (not packet contents) for network traffic in a VPC.

- **Attachment Levels**:

  - **VPC level**: Logs all ENIs in the VPC.

  - **Subnet level**: Logs all ENIs in the subnet.

  - **ENI level**: Logs traffic specific to an individual ENI.

- **Traffic Types**: Can log ACCEPTED, REJECTED, or ALL traffic.

- **Log Destinations**:

  - **Amazon S3**: Long-term storage.

  - **CloudWatch Logs**: Real-time monitoring and alerting.

  - **Amazon Athena**: Interactive querying of logs.

- **Limitations**:

  - Logs are not real-time (processing delay).

  - Certain traffic is excluded by default (e.g., AWS DNS queries, traffic to/from the instance metadata service).

- **Use Cases**: Troubleshooting, compliance, network traffic analysis, and security auditing.
