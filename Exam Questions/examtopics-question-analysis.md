## Question #01

A gaming website gives users the ability to trade game items with each other on the platform. The platform requires both users' records to be updated and persisted in one transaction. If any update fails, the transaction must roll back.
Which AWS solution can provide the transactional capability that is required for this feature?

- A. Amazon DynamoDB with operations made with the Consistent Read parameter set to true
- B. Amazon ElastiCache for Memcached with operations made within a transaction block
- C. Amazon DynamoDB with reads and writes made by using TransactGetItems and TransactWriteItems operations
- D. Amazon Aurora MySQL with operations made within a transaction block
- E. Amazon Athena with operations made within a transaction block

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Amazon DynamoDB with reads and writes made by using TransactGetItems and TransactWriteItems operations**

### Explanation

- **Amazon DynamoDB** supports **TransactWriteItems** and **TransactGetItems** operations, which allow you to perform multiple reads and writes as a single, atomic transaction. This ensures that either all the operations in the transaction succeed, or none of them do, providing the transactional capability required for the gaming platform's item trading feature.
- **Option A** is incorrect because the "Consistent Read" parameter ensures strong consistency for individual read operations but does not provide transactional capabilities for multiple operations.
- **Option B** is incorrect because Amazon ElastiCache for Memcached does not support transactional operations.
- **Option D** is incorrect because while Amazon Aurora MySQL supports transactions, it is not the best fit for a gaming platform that likely requires high scalability and low latency, which DynamoDB is better suited for.
- **Option E** is incorrect because Amazon Athena is a query service for analyzing data in S3 and does not support transactional operations.

</details>

## Question #02

A developer has created a Java application that makes HTTP requests directly to AWS services. Application logging shows 5xx HTTP response codes that occur at irregular intervals. The errors are affecting users.
How should the developer update the application to improve the application's resiliency?

- A. Revise the request content in the application code.
- B. Use the AWS SDK for Java to interact with AWS APIs.
- C. Scale out the application so that more instances of the application are running.
- D. Add additional logging to the application code.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Use the AWS SDK for Java to interact with AWS APIs.**

### Explanation

- **Option B** is the best choice because the AWS SDK for Java is designed to handle AWS service interactions more robustly, including built-in retry mechanisms and error handling for transient issues like 5xx errors. This improves the application's resiliency without requiring manual intervention.
- **Option A** is incorrect because revising the request content does not address the root cause of 5xx errors, which are typically server-side issues.
- **Option C** is incorrect because scaling out the application does not resolve the underlying issue of 5xx errors. It may distribute the load but does not improve the handling of transient errors.
- **Option D** is incorrect because while additional logging can help diagnose issues, it does not improve the application's ability to handle or recover from 5xx errors.

Using the AWS SDK for Java ensures better error handling and retries, making the application more resilient to transient failures.

</details>

## Question #03

A global company has a mobile app with static data stored in an Amazon S3 bucket in the us-east-1 Region. The company serves the content through an Amazon
CloudFront distribution. The company is launching the mobile app in South Africa. The data must reside in the af-south-1 Region. The company does not want to deploy a specific mobile client for South Africa.
What should the company do to meet these requirements?

- A. Use the CloudFront geographic restriction feature to block access to users in South Africa.
- B. Create a Lambda@Edge function. Associate the Lambda@Edge function as an origin request trigger with the CloudFront distribution to change the S3 origin Region.
- C. Create a Lambda@Edge function. Associate the Lambda@Edge function as a viewer response trigger with the CloudFront distribution to change the S3 origin Region.
- D. Include af-south-1 in the alternate domain name (CNAME) of the CloudFront distribution.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create a Lambda@Edge function. Associate the Lambda@Edge function as an origin request trigger with the CloudFront distribution to change the S3 origin Region.**

### Explanation

- **Option B** is correct because a Lambda@Edge function can dynamically change the origin (S3 bucket) based on the user's location. By associating the function as an **origin request trigger**, the function can redirect requests from users in South Africa to the S3 bucket in the `af-south-1` Region, ensuring the data resides in the required Region without deploying a specific mobile client for South Africa.
- **Option A** is incorrect because blocking access to users in South Africa does not meet the requirement of serving content to users in that region.
- **Option C** is incorrect because a **viewer response trigger** occurs after the origin has already been accessed, which is too late to change the S3 origin Region.
- **Option D** is incorrect because adding `af-south-1` to the alternate domain name (CNAME) does not change the origin Region or ensure that the data resides in `af-south-1`.

Using a Lambda@Edge function as an **origin request trigger** is the most effective solution to dynamically route users to the appropriate S3 bucket based on their geographic location.

</details>

## Question #04

A developer is testing an AWS Lambda function by using the AWS Serverless Application Model (AWS SAM) local CLI. The application that is implemented by the Lambda function makes several AWS API calls by using the AWS software development kit (SDK). The developer wants to allow the function to make AWS API calls in a test AWS account from the developer's laptop.  
What should the developer do to meet these requirements?

- A. Edit the `template.yml` file. Add the `AWS_ACCESS_KEY_ID` property and the `AWS_SECRET_ACCESS_KEY` property in the Globals section.
- B. Add a test profile by using the `aws configure` command with the `--profile` option. Run AWS SAM by using the `sam local invoke` command with the `--profile` option.
- C. Edit the `template.yml` file. For the `AWS::Serverless::Function` resource, set the role to an IAM role in the AWS account.
- D. Run the function by using the `sam local invoke` command. Override the `AWS_ACCESS_KEY_ID` parameter and the `AWS_SECRET_ACCESS_KEY` parameter by specifying the `--parameter-overrides` option.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Add a test profile by using the `aws configure` command with the `--profile` option. Run AWS SAM by using the `sam local invoke` command with the `--profile` option.**

### Explanation

- **Option B** is correct because it allows the developer to configure AWS credentials locally using the AWS CLI (`aws configure --profile`) and then use those credentials when invoking the Lambda function locally with the `sam local invoke --profile` command. This ensures that the Lambda function can make AWS API calls using the test AWS account credentials.
- **Option A** is incorrect because hardcoding credentials in the `template.yml` file is not a secure practice and is not recommended.
- **Option C** is incorrect because setting an IAM role in the `template.yml` file does not apply to local testing. Local testing requires explicit credentials to be provided.
- **Option D** is incorrect because the `--parameter-overrides` option is used to override CloudFormation template parameters, not to provide AWS credentials for local testing.

Using the `aws configure` command and the `--profile` option with `sam local invoke` is the recommended and secure way to test AWS API calls locally.

</details>

## Question #05

A developer designed an application on an Amazon EC2 instance. The application makes API requests to objects in an Amazon S3 bucket.  
Which combination of steps will ensure that the application makes the API requests in the MOST secure manner? (Choose two.)

- A. Create an IAM user that has permissions to the S3 bucket. Add the user to an IAM group.
- B. Create an IAM role that has permissions to the S3 bucket.
- C. Add the IAM role to an instance profile. Attach the instance profile to the EC2 instance.
- D. Create an IAM role that has permissions to the S3 bucket. Assign the role to an IAM group.
- E. Store the credentials of the IAM user in the environment variables on the EC2 instance.

<details>
<summary>Answer</summary>
<br>

The correct answers are:

**B. Create an IAM role that has permissions to the S3 bucket.**  
**C. Add the IAM role to an instance profile. Attach the instance profile to the EC2 instance.**

### Explanation

- **Option B** is correct because creating an IAM role with permissions to the S3 bucket allows the EC2 instance to securely access the bucket without hardcoding credentials. Roles are the recommended way to grant permissions to AWS services.
- **Option C** is correct because attaching the IAM role to an instance profile and associating it with the EC2 instance ensures that the application running on the instance can securely assume the role and access the S3 bucket.
- **Option A** is incorrect because creating an IAM user and adding it to a group is not the most secure approach for EC2 instances. Hardcoding or storing user credentials on the instance is not recommended.
- **Option D** is incorrect because IAM roles cannot be assigned to IAM groups. Roles are meant to be assumed by AWS services, users, or applications.
- **Option E** is incorrect because storing IAM user credentials in environment variables is insecure and violates AWS best practices. Credentials can be exposed or compromised.

Using an IAM role and attaching it to the EC2 instance via an instance profile is the most secure and recommended approach for granting permissions to AWS resources.

</details>

## Question #06

A developer is configuring an Amazon CloudFront distribution for a new application to provide encryption in transit. The application is running in the eu-west-1 Region. The developer creates a new certificate in AWS Certificate Manager (ACM) in eu-west-1, but the certificate is not visible in the CloudFront distribution settings.  
What should the developer do to fix this problem?

- A. Create the certificate for the domain in the same Region as the application. Ensure that the alternate domain name (CNAME) in the distribution settings matches the domain name in the certificate.
- B. Create the certificate in the us-east-1 Region. Ensure that the alternate domain name (CNAME) in the distribution settings matches the domain name in the certificate.
- C. Recreate the CloudFront distribution in the same Region as the certificate.
- D. Specify the ACM certificate name as the default root object of the CloudFront distribution.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create the certificate in the us-east-1 Region. Ensure that the alternate domain name (CNAME) in the distribution settings matches the domain name in the certificate.**

### Explanation

- **Option B** is correct because **CloudFront requires certificates to be created in the `us-east-1` Region**. Certificates created in other Regions (like `eu-west-1`) will not be visible or usable in CloudFront. Additionally, the alternate domain name (CNAME) in the CloudFront distribution settings must match the domain name specified in the certificate.
- **Option A** is incorrect because creating the certificate in the same Region as the application (`eu-west-1`) will not work. CloudFront only supports certificates from the `us-east-1` Region.
- **Option C** is incorrect because CloudFront distributions are global and not tied to a specific Region. Recreating the distribution in the same Region as the certificate will not resolve the issue.
- **Option D** is incorrect because the default root object is used to specify the default file to serve when a user accesses the root URL of the distribution. It has no relation to ACM certificates.

To fix the issue, the developer must create the certificate in the `us-east-1` Region and ensure the alternate domain name (CNAME) matches the domain name in the certificate.

</details>

## Question #07

A developer is building an application that runs behind an Application Load Balancer (ALB). The ALB is configured as the origin for an Amazon CloudFront distribution. Users will log in to the application by using their social media accounts.  
How can the developer authenticate users?

- A. Validate the users by inspecting the tokens in an AWS Lambda authorizer on the ALB.
- B. Configure the ALB to use Amazon Cognito as one of the authentication providers.
- C. Configure CloudFront to use Amazon Cognito as one of the authentication providers.
- D. Validate the users by calling the Amazon Cognito API in an AWS Lambda authorizer on the ALB.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Configure the ALB to use Amazon Cognito as one of the authentication providers.**

### Explanation

- **Option B** is correct because **Application Load Balancers (ALBs) natively support integration with Amazon Cognito** for user authentication. Amazon Cognito can be configured as an authentication provider, allowing users to log in using their social media accounts (e.g., Google, Facebook, etc.). The ALB handles the authentication process before forwarding requests to the application.
- **Option A** is incorrect because ALBs do not support AWS Lambda authorizers. Lambda authorizers are used with API Gateway, not ALBs.
- **Option C** is incorrect because CloudFront does not natively support Amazon Cognito for authentication. Authentication is typically handled at the ALB or application level.
- **Option D** is incorrect because ALBs do not support Lambda authorizers, and this approach would add unnecessary complexity. The ALB's native integration with Amazon Cognito is the recommended and simpler solution.

By configuring the ALB to use Amazon Cognito, the developer can seamlessly authenticate users via their social media accounts without additional custom code.

</details>

## Question #08

A company has an application that analyzes photographs. A developer is preparing the application for deployment to Amazon EC2 instances. The application's image analysis functions require a mix of GPU instances and CPU instances that run on Amazon Linux. The developer needs to add code to the application so that the functions can determine whether they are running on a GPU instance.  
What should the functions do to obtain this information?

- A. Call the `DescribeInstances` API operation and filter on the current instance ID. Examine the `ElasticGpuAssociations` property.
- B. Evaluate the `GPU_AVAILABLE` environment variable.
- C. Call the `DescribeElasticGpus` API operation.
- D. Retrieve the instance type from the instance metadata.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Retrieve the instance type from the instance metadata.**

### Explanation

- **Option D** is correct because the **instance metadata service** provides information about the EC2 instance, including the instance type. By retrieving the instance type (e.g., `p2.xlarge`, `g4dn.xlarge`, etc.), the application can determine whether it is running on a GPU instance. GPU instances have specific instance type prefixes like `p`, `g`, or `inf`.
- **Option A** is incorrect because the `ElasticGpuAssociations` property is not used to determine whether an instance is a GPU instance. It is related to Elastic GPUs, which are a different feature and not commonly used.
- **Option B** is incorrect because there is no standard `GPU_AVAILABLE` environment variable provided by Amazon Linux or EC2.
- **Option C** is incorrect because the `DescribeElasticGpus` API operation is used to describe Elastic GPUs, not GPU instances. This is not relevant for determining the instance type.

The simplest and most reliable way to determine if the application is running on a GPU instance is to retrieve the instance type from the instance metadata service.

</details>

## Question #09

A company has an application that uses Amazon Cognito user pools as an identity provider. The company must secure access to user records. The company has set up multi-factor authentication (MFA). The company also wants to send a login activity notification by email every time a user logs in.  
What is the MOST operationally efficient solution that meets this requirement?

- A. Create an AWS Lambda function that uses Amazon Simple Email Service (Amazon SES) to send the email notification. Add an Amazon API Gateway API to invoke the function. Call the API from the client side when login confirmation is received.
- B. Create an AWS Lambda function that uses Amazon Simple Email Service (Amazon SES) to send the email notification. Add an Amazon Cognito post authentication Lambda trigger for the function.
- C. Create an AWS Lambda function that uses Amazon Simple Email Service (Amazon SES) to send the email notification. Create an Amazon CloudWatch Logs log subscription filter to invoke the function based on the login status.
- D. Configure Amazon Cognito to stream all logs to Amazon Kinesis Data Firehose. Create an AWS Lambda function to process the streamed logs and to send the email notification based on the login status of each user.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create an AWS Lambda function that uses Amazon Simple Email Service (Amazon SES) to send the email notification. Add an Amazon Cognito post authentication Lambda trigger for the function.**

### Explanation

- **Option B** is correct because **Amazon Cognito supports Lambda triggers**, including the **post-authentication trigger**, which is invoked after a successful user login. This is the most operationally efficient solution because it directly integrates with Cognito's authentication flow, eliminating the need for additional services or manual API calls.
- **Option A** is incorrect because it requires additional complexity, such as setting up an API Gateway and making client-side API calls, which is less efficient and more error-prone.
- **Option C** is incorrect because using CloudWatch Logs and log subscription filters introduces unnecessary latency and complexity. It is not the most direct or efficient way to handle login notifications.
- **Option D** is incorrect because streaming logs to Kinesis Data Firehose and processing them with Lambda is overly complex for this use case. It introduces additional cost and latency compared to using a Cognito Lambda trigger.

Using a **post-authentication Lambda trigger** with Amazon Cognito is the simplest, most efficient, and most reliable way to send login activity notifications.

</details>

## Question #10

A company hosts a three-tier web application on AWS behind an Amazon CloudFront distribution. A developer wants a dashboard to monitor error rates and anomalies of the CloudFront distribution with the shortest possible refresh interval.  
Which combination of steps should the developer take to meet these requirements? (Choose two.)

- A. Activate real-time logs on the CloudFront distribution. Create a stream in Amazon Kinesis Data Streams.
- B. Export the CloudFront logs to an Amazon S3 bucket. Detect anomalies and error rates with Amazon QuickSight.
- C. Configure Amazon Kinesis Data Streams to deliver logs to Amazon OpenSearch Service (Amazon Elasticsearch Service). Create a dashboard in OpenSearch Dashboards (Kibana).
- D. Create Amazon CloudWatch alarms based on expected values of selected CloudWatch metrics to detect anomalies and errors.
- E. Design an Amazon CloudWatch dashboard of the selected CloudFront distribution metrics.

<details>
<summary>Answer</summary>
<br>

The correct answers are:

**D. Create Amazon CloudWatch alarms based on expected values of selected CloudWatch metrics to detect anomalies and errors.**  
**E. Design an Amazon CloudWatch dashboard of the selected CloudFront distribution metrics.**

### Explanation

#### **Option D: Create Amazon CloudWatch alarms based on expected values of selected CloudWatch metrics to detect anomalies and errors.**

- **Purpose of Amazon CloudWatch**: Amazon CloudWatch is a monitoring and observability service that collects and tracks metrics, logs, and events from AWS resources and applications. It provides real-time insights into system performance and operational health.
- **Why this option is correct**: CloudFront automatically sends metrics (e.g., `4xxErrorRate`, `5xxErrorRate`, `TotalErrorRate`) to CloudWatch. By creating CloudWatch alarms, the developer can set thresholds for these metrics to detect anomalies and errors in real time. Alarms can trigger notifications or automated actions when thresholds are breached, ensuring quick response to issues.
- **Refresh interval**: CloudWatch metrics are available at a 1-minute granularity, providing near real-time monitoring.

#### **Option E: Design an Amazon CloudWatch dashboard of the selected CloudFront distribution metrics.**

- **Purpose of Amazon CloudWatch Dashboards**: CloudWatch Dashboards allow you to create customizable visualizations of metrics and alarms. They provide a centralized view of the health and performance of your AWS resources.
- **Why this option is correct**: By designing a CloudWatch dashboard, the developer can visualize key CloudFront metrics (e.g., error rates, request counts, latency) in real time. This provides a comprehensive view of the distribution's performance and helps identify trends or anomalies quickly.
- **Refresh interval**: CloudWatch Dashboards can refresh metrics every minute, ensuring the shortest possible refresh interval for monitoring.

#### **Why other options are incorrect**

- **Option A**: Activating real-time logs and using Amazon Kinesis Data Streams is useful for detailed log analysis but is not the most efficient way to monitor error rates and anomalies in real time. It involves additional complexity and cost compared to using CloudWatch metrics.
- **Option B**: Exporting logs to Amazon S3 and using Amazon QuickSight is suitable for historical analysis and reporting but does not provide real-time monitoring capabilities.
- **Option C**: Using Amazon Kinesis Data Streams and Amazon OpenSearch Service (Elasticsearch) is powerful for log analysis and visualization but is overkill for this use case. It introduces unnecessary complexity and latency compared to CloudWatch.

#### **Key Takeaways**

- **CloudWatch Metrics**: Automatically collected by CloudFront and available at 1-minute granularity.
- **CloudWatch Alarms**: Provide real-time anomaly and error detection with automated notifications.
- **CloudWatch Dashboards**: Offer real-time visualization of metrics with a 1-minute refresh interval.

By combining **CloudWatch alarms** and **CloudWatch dashboards**, the developer can achieve real-time monitoring of error rates and anomalies with the shortest possible refresh interval.

</details>
