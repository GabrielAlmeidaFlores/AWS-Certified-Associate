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

Represents a single data entry in a metric, consisting of:

- **Timestamp**: The time the data point was recorded.

- **Value**: The metric value at the specified timestamp.

- **(Optional) Unit of Measure**: Describes the metric's unit, such as Percent or Bytes.

#### Metrics

Metrics are the fundamental concept in CloudWatch. A metric represents a time-ordered set of data points that are published to CloudWatch. Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time. For example, the CPU usage of a particular EC2 instance is one metric provided by Amazon EC2.

The data points themselves can come from any application or business activity from which you collect data. Metrics exist only in the Region in which they are created. Metrics cannot be deleted, but they automatically expire after 15 months if no new data is published to them. Data points older than 15 months expire on a rolling basis; as new data points come in, data older than 15 months is dropped.

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

#### Metrics retention

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
