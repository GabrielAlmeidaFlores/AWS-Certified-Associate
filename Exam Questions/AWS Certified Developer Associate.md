## Question #01

A gaming website gives users the ability to trade game items with each other on the platform. The platform requires both users' records to be updated and persisted in one transaction. If any update fails, the transaction must roll back.  
Which AWS solution can provide the transactional capability that is required for this feature?

- A. Amazon DynamoDB with operations made with the Consistent Read parameter set to true
- B. Amazon ElastiCache for Memcached with operations made within a transaction block
- C. Amazon DynamoDB with reads and writes made by using `TransactGetItems` and `TransactWriteItems` operations
- D. Amazon Aurora MySQL with operations made within a transaction block
- E. Amazon Athena with operations made within a transaction block

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Amazon DynamoDB with reads and writes made by using `TransactGetItems` and `TransactWriteItems` operations**

### Explanation

#### **Purpose of Transactional Capability**

The gaming platform requires **atomicity** for trading game items, meaning that updates to both users' records must succeed or fail together. If any part of the transaction fails, the entire transaction must roll back.

#### **Why this option is correct**

- **Amazon DynamoDB Transactions**: DynamoDB supports **transactions** through the `TransactGetItems` and `TransactWriteItems` APIs. These APIs allow multiple read and write operations to be grouped into a single, atomic transaction.
- **Atomicity**: If any operation in the transaction fails, the entire transaction is rolled back, ensuring data consistency.
- **Scalability**: DynamoDB is a fully managed NoSQL database that provides high scalability and performance, making it suitable for gaming applications with high transaction volumes.

#### **Why other options are incorrect**

- **Option A**: The `Consistent Read` parameter ensures strong consistency for individual read operations but does not provide transactional capabilities for multiple operations.
- **Option B**: Amazon ElastiCache for Memcached does not support transactions. It is a caching service and is not designed for transactional workloads.
- **Option D**: Amazon Aurora MySQL supports transactions, but it is a relational database and may not scale as well as DynamoDB for high-throughput gaming applications.
- **Option E**: Amazon Athena is a query service for analyzing data in S3 and does not support transactional operations.

#### **Key Takeaways**

- **DynamoDB Transactions**: Use `TransactGetItems` and `TransactWriteItems` to perform atomic transactions in DynamoDB.
- **Atomicity**: Ensure that all operations in the transaction succeed or fail together.
- **Scalability**: DynamoDB is well-suited for high-throughput, low-latency applications like gaming platforms.

By using **Amazon DynamoDB with `TransactGetItems` and `TransactWriteItems`**, the developer can implement the required transactional capability for trading game items on the platform.

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

#### **Purpose of Improving Resiliency**

The application is experiencing 5xx errors, which indicate server-side issues (e.g., throttling, service disruptions). To improve resiliency, the application should handle these errors gracefully and retry failed requests.

#### **Why this option is correct**

- **AWS SDK for Java**: The AWS SDK provides built-in **retry mechanisms** and **error handling** for AWS API calls. It automatically retries failed requests with exponential backoff, reducing the impact of transient errors (e.g., 5xx errors).
- **Best Practices**: The SDK follows AWS best practices for interacting with services, including handling throttling, timeouts, and service disruptions.
- **Simplified Development**: Using the SDK simplifies development by abstracting low-level HTTP requests and providing higher-level APIs for interacting with AWS services.

#### **Why other options are incorrect**

- **Option A**: Revising the request content does not address the root cause of 5xx errors, which are server-side issues.
- **Option C**: Scaling out the application does not resolve the issue of 5xx errors. It may distribute the load but does not improve error handling or retry logic.
- **Option D**: Additional logging helps diagnose issues but does not improve the application's ability to handle or recover from errors.

#### **Key Takeaways**

- **AWS SDK**: Use the AWS SDK for Java to benefit from built-in retry mechanisms and error handling.
- **Resiliency**: The SDK improves application resiliency by automatically retrying failed requests and handling transient errors.
- **Best Practices**: The SDK follows AWS best practices, ensuring secure and efficient interactions with AWS services.

By using the **AWS SDK for Java**, the developer can improve the application's resiliency and reduce the impact of 5xx errors on users.

</details>

## Question #03

A global company has a mobile app with static data stored in an Amazon S3 bucket in the us-east-1 Region. The company serves the content through an Amazon CloudFront distribution. The company is launching the mobile app in South Africa. The data must reside in the af-south-1 Region. The company does not want to deploy a specific mobile client for South Africa.  
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

#### **Purpose of Regional Data Residency**

The company needs to ensure that data for South African users resides in the `af-south-1` Region while continuing to serve global users from the `us-east-1` Region. This requires dynamically routing requests based on the user's location.

#### **Why this option is correct**

- **Lambda@Edge**: Lambda@Edge allows you to run code at CloudFront edge locations, enabling dynamic request handling based on user location.
- **Origin Request Trigger**: By associating a Lambda@Edge function as an **origin request trigger**, the function can inspect the request and modify the origin (S3 bucket) based on the user's geographic location.
- **Dynamic Routing**: The Lambda@Edge function can route requests from South African users to the S3 bucket in `af-south-1` and route all other requests to the S3 bucket in `us-east-1`.

#### **Why other options are incorrect**

- **Option A**: Blocking access to users in South Africa does not meet the requirement of serving data from the `af-south-1` Region.
- **Option C**: A viewer response trigger occurs after the origin has been accessed, which is too late to change the S3 origin Region.
- **Option D**: Including `af-south-1` in the alternate domain name (CNAME) does not change the origin Region or ensure that data resides in `af-south-1`.

#### **Key Takeaways**

- **Lambda@Edge**: Use Lambda@Edge to dynamically route requests based on user location.
- **Origin Request Trigger**: Modify the origin (S3 bucket) at the edge based on the user's geographic location.
- **Regional Data Residency**: Ensure that data for South African users resides in the `af-south-1` Region while serving global users from `us-east-1`.

By creating a **Lambda@Edge function** and associating it as an **origin request trigger**, the company can dynamically route requests to the appropriate S3 bucket based on the user's location, meeting the data residency requirements.

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

#### **Purpose of Testing AWS API Calls Locally**

The developer needs to test the Lambda function locally while allowing it to make AWS API calls using the AWS SDK. This requires configuring AWS credentials for the test AWS account.

#### **Why this option is correct**

- **AWS CLI Configuration**: The `aws configure` command allows the developer to set up a profile with AWS credentials (access key and secret key) for the test AWS account.
- **Profile Usage**: By using the `--profile` option with the `sam local invoke` command, the developer can specify the profile to use for AWS API calls during local testing.
- **Secure and Flexible**: This approach avoids hardcoding credentials in the `template.yml` file and allows the developer to switch between different profiles for different environments (e.g., test, production).

#### **Why other options are incorrect**

- **Option A**: Hardcoding credentials in the `template.yml` file is not secure and is not recommended. Credentials should be managed separately using the AWS CLI or environment variables.
- **Option C**: Setting an IAM role in the `template.yml` file does not apply to local testing. Local testing requires explicit credentials to be provided.
- **Option D**: The `--parameter-overrides` option is used to override CloudFormation template parameters, not to provide AWS credentials for local testing.

#### **Key Takeaways**

- **AWS CLI Configuration**: Use `aws configure` to set up a profile with AWS credentials for the test AWS account.
- **Profile Usage**: Use the `--profile` option with `sam local invoke` to specify the profile for AWS API calls during local testing.
- **Security**: Avoid hardcoding credentials in the `template.yml` file or other configuration files.

By adding a **test profile** using the `aws configure` command and running AWS SAM with the `--profile` option, the developer can securely test the Lambda function locally while allowing it to make AWS API calls.

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

#### **Purpose of Secure API Requests**

To securely access S3 from an EC2 instance, the application should use temporary credentials provided by an IAM role, rather than hardcoding or storing long-term credentials.

#### **Why these options are correct**

1. **IAM Role**: An IAM role defines a set of permissions for accessing AWS resources (e.g., S3). Roles are more secure than IAM users because they provide temporary credentials that are automatically rotated.
2. **Instance Profile**: An instance profile is a container for an IAM role that can be attached to an EC2 instance. When the role is attached to the instance, the application running on the instance can use the role's permissions to access S3.
3. **Temporary Credentials**: The EC2 instance automatically retrieves temporary credentials from the instance profile, eliminating the need to store long-term credentials (e.g., access keys) on the instance.

#### **Why other options are incorrect**

- **Option A**: Creating an IAM user and adding it to a group is not the most secure approach. IAM users have long-term credentials, which can be compromised if stored on the EC2 instance.
- **Option D**: IAM roles cannot be assigned to IAM groups. Roles are meant to be assumed by AWS services, users, or applications.
- **Option E**: Storing IAM user credentials in environment variables is insecure. Long-term credentials can be exposed or compromised, posing a security risk.

#### **Key Takeaways**

- **IAM Roles**: Use IAM roles to grant permissions to EC2 instances securely.
- **Instance Profiles**: Attach the IAM role to an instance profile and associate it with the EC2 instance.
- **Temporary Credentials**: Temporary credentials are automatically managed and rotated, providing a more secure solution than long-term credentials.

By creating an **IAM role** and attaching it to the EC2 instance via an **instance profile**, the developer can ensure that the application makes API requests to S3 in the most secure manner.

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

#### **Purpose of Encryption in Transit**

To provide encryption in transit for the CloudFront distribution, the developer needs to use an SSL/TLS certificate from AWS Certificate Manager (ACM). However, CloudFront only supports certificates created in the **us-east-1** Region.

#### **Why this option is correct**

- **ACM Certificate in us-east-1**: CloudFront requires SSL/TLS certificates to be created in the **us-east-1** Region, regardless of where the application or CloudFront distribution is located.
- **Alternate Domain Name (CNAME)**: The alternate domain name in the CloudFront distribution settings must match the domain name specified in the ACM certificate.
- **Fix the Issue**: By creating the certificate in the **us-east-1** Region and ensuring the CNAME matches, the developer can successfully configure encryption in transit for the CloudFront distribution.

#### **Why other options are incorrect**

- **Option A**: Creating the certificate in the same Region as the application (eu-west-1) will not work, as CloudFront only supports certificates from the **us-east-1** Region.
- **Option C**: Recreating the CloudFront distribution in the same Region as the certificate (eu-west-1) will not resolve the issue, as CloudFront still requires the certificate to be in **us-east-1**.
- **Option D**: The default root object is used to specify the default file to serve when a user accesses the root URL of the distribution. It has no relation to ACM certificates.

#### **Key Takeaways**

- **ACM Certificate Region**: CloudFront only supports SSL/TLS certificates created in the **us-east-1** Region.
- **Alternate Domain Name**: Ensure the alternate domain name in the CloudFront distribution matches the domain name in the ACM certificate.
- **Encryption in Transit**: Use an ACM certificate to enable HTTPS for the CloudFront distribution.

By creating the **ACM certificate in the us-east-1 Region** and ensuring the **alternate domain name matches**, the developer can fix the issue and enable encryption in transit for the CloudFront distribution.

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

#### **Purpose of User Authentication**

The application needs to authenticate users who log in using their social media accounts. This requires integrating a secure and scalable authentication solution.

#### **Why this option is correct**

- **Amazon Cognito**: Cognito is a fully managed service that provides user authentication and authorization. It supports social identity providers (e.g., Google, Facebook) for user login.
- **ALB Integration**: The ALB can be configured to use Amazon Cognito as an authentication provider. When a user tries to access the application, the ALB redirects them to Cognito for authentication. After successful authentication, the ALB allows access to the application.
- **Seamless Integration**: This approach provides a seamless and secure way to authenticate users without requiring custom code or additional infrastructure.

#### **Why other options are incorrect**

- **Option A**: ALBs do not support Lambda authorizers. Lambda authorizers are used with API Gateway, not ALBs.
- **Option C**: CloudFront does not natively support Amazon Cognito for authentication. Authentication is typically handled at the ALB or application level.
- **Option D**: ALBs do not support Lambda authorizers, and this approach would add unnecessary complexity. The ALB's native integration with Amazon Cognito is the recommended solution.

#### **Key Takeaways**

- **Amazon Cognito**: Use Cognito for user authentication with social identity providers.
- **ALB Integration**: Configure the ALB to use Cognito as an authentication provider for seamless user login.
- **Secure and Scalable**: Cognito provides a secure and scalable solution for user authentication.

By configuring the **ALB to use Amazon Cognito**, the developer can authenticate users who log in using their social media accounts.

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

#### **Purpose of Instance Type Detection**

The application needs to determine whether it is running on a GPU instance to execute the appropriate image analysis functions. This requires identifying the instance type.

#### **Why this option is correct**

- **Instance Metadata**: EC2 instances provide metadata that includes information about the instance, such as the instance type. The metadata can be accessed at `http://169.254.169.254/latest/meta-data/`.
- **Instance Type**: By retrieving the instance type from the metadata, the application can determine whether it is running on a GPU instance (e.g., `p2`, `p3`, `g4dn`, etc.).
- **Simple and Efficient**: Accessing instance metadata is a simple and efficient way to determine the instance type without making external API calls.

#### **Why other options are incorrect**

- **Option A**: The `DescribeInstances` API operation is used to retrieve information about EC2 instances but requires API calls and permissions. It is not necessary for this use case.
- **Option B**: There is no standard `GPU_AVAILABLE` environment variable provided by Amazon Linux or EC2.
- **Option C**: The `DescribeElasticGpus` API operation is used to describe Elastic GPUs, which are a different feature and not relevant for determining the instance type.

#### **Key Takeaways**

- **Instance Metadata**: Use the instance metadata service to retrieve the instance type.
- **GPU Instances**: GPU instances have specific instance type prefixes (e.g., `p`, `g`, `inf`).
- **Efficiency**: Accessing instance metadata is the most efficient way to determine the instance type.

By retrieving the **instance type from the instance metadata**, the application can determine whether it is running on a GPU instance and execute the appropriate image analysis functions.

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

#### **Purpose of Login Activity Notifications**

The company wants to send an email notification every time a user logs in. This requires triggering an action immediately after a successful login.

#### **Why this option is correct**

- **Post Authentication Trigger**: Amazon Cognito supports **Lambda triggers**, including the **post authentication trigger**. This trigger is invoked after a user successfully authenticates, making it the perfect place to send login activity notifications.
- **Lambda Function**: The Lambda function can use Amazon SES to send an email notification to the user or an administrator.
- **Operationally Efficient**: This approach is the most efficient because it directly integrates with Cognito's authentication flow and does not require additional services or manual API calls.

#### **Why other options are incorrect**

- **Option A**: Using API Gateway and client-side API calls adds unnecessary complexity. It is not as efficient as using a Cognito Lambda trigger.
- **Option C**: Using CloudWatch Logs and log subscription filters introduces latency and complexity. It is not as efficient as using a Cognito Lambda trigger.
- **Option D**: Streaming logs to Kinesis Data Firehose and processing them with Lambda is overly complex for this use case. It introduces additional latency and cost.

#### **Key Takeaways**

- **Cognito Lambda Triggers**: Use Cognito's post authentication trigger to execute actions immediately after a user logs in.
- **Lambda Function**: Create a Lambda function to send email notifications using Amazon SES.
- **Operational Efficiency**: Directly integrating with Cognito's authentication flow is the most efficient way to send login activity notifications.

By creating a **Lambda function** and adding it as a **post authentication trigger** in Amazon Cognito, the company can efficiently send login activity notifications by email.

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

**A. Activate real-time logs on the CloudFront distribution. Create a stream in Amazon Kinesis Data Streams.**  
**C. Configure Amazon Kinesis Data Streams to deliver logs to Amazon OpenSearch Service (Amazon Elasticsearch Service). Create a dashboard in OpenSearch Dashboards (Kibana).**

### Explanation

#### **Option A: Activate real-time logs on the CloudFront distribution. Create a stream in Amazon Kinesis Data Streams.**

- **Purpose of Real-Time Logs**: Real-time logs for CloudFront provide access to log data with a latency of just a few seconds, enabling near real-time monitoring of requests and errors.
- **Purpose of Amazon Kinesis Data Streams**: Kinesis Data Streams is a scalable and durable real-time data streaming service that can ingest, process, and deliver large volumes of data.
- **Why this option is correct**: By activating real-time logs and streaming them to Kinesis Data Streams, the developer can achieve the shortest possible refresh interval for monitoring. This setup ensures that log data is available for analysis almost immediately after it is generated.

#### **Option C: Configure Amazon Kinesis Data Streams to deliver logs to Amazon OpenSearch Service (Amazon Elasticsearch Service). Create a dashboard in OpenSearch Dashboards (Kibana).**

- **Purpose of Amazon OpenSearch Service**: OpenSearch Service (formerly Amazon Elasticsearch Service) is a fully managed service that makes it easy to deploy, operate, and scale OpenSearch clusters for log analytics and search.
- **Purpose of OpenSearch Dashboards (Kibana)**: OpenSearch Dashboards (or Kibana) is a visualization tool that allows you to create interactive dashboards and perform real-time analysis on log data.
- **Why this option is correct**: By delivering logs from Kinesis Data Streams to OpenSearch Service, the developer can create a real-time dashboard in OpenSearch Dashboards (Kibana) to monitor error rates and anomalies. This setup provides a powerful and flexible way to visualize and analyze log data with minimal latency.

#### **Why other options are incorrect**

- **Option B**: Exporting logs to Amazon S3 and using Amazon QuickSight is suitable for historical analysis and reporting but does not provide real-time monitoring capabilities. QuickSight is not designed for real-time dashboards with the shortest refresh interval.
- **Option D**: Creating CloudWatch alarms is useful for real-time anomaly detection but does not provide a dashboard for visualizing error rates and trends. CloudWatch metrics also have a 1-minute granularity, which may not be as fast as real-time logs.
- **Option E**: Designing a CloudWatch dashboard is useful for visualizing metrics but does not provide the same level of real-time analysis as OpenSearch Dashboards (Kibana) with real-time logs.

#### **Key Takeaways**

- **Real-Time Logs**: Provide the shortest possible refresh interval for monitoring CloudFront error rates and anomalies.
- **Kinesis Data Streams**: Enables real-time ingestion and delivery of log data.
- **OpenSearch Service and Dashboards**: Provide a powerful platform for real-time log analysis and visualization.

By combining **real-time logs**, **Kinesis Data Streams**, and **OpenSearch Service**, the developer can create a real-time dashboard to monitor error rates and anomalies with the shortest possible refresh interval.

</details>

## Question #11

A developer creates a customer managed key for multiple AWS users to encrypt data in Amazon S3. The developer configures Amazon Simple Notification Service (Amazon SNS) to publish a message if key deletion is scheduled. The developer needs to preserve any SNS messages that cannot be delivered so that those messages can be reprocessed.  
Which AWS service or feature should the developer use to meet this requirement?

- A. Amazon Simple Email Service (Amazon SES)
- B. AWS Lambda
- C. Amazon Simple Queue Service (Amazon SQS)
- D. Amazon CloudWatch alarm

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Amazon Simple Queue Service (Amazon SQS)**

### Explanation

#### **Purpose of Amazon SQS**

Amazon SQS is a fully managed message queuing service that enables decoupling and asynchronous communication between distributed systems. It allows messages to be stored temporarily until they are processed by a consumer.

#### **Why this option is correct**

- The developer needs to ensure that **undelivered SNS messages are preserved** for reprocessing. SNS does not store messages if they cannot be delivered to subscribers. To address this, the developer can configure an **SQS queue as a subscriber to the SNS topic**.
- When SNS publishes a message, it will be delivered to the SQS queue. If the message cannot be processed immediately, it will remain in the queue until a consumer retrieves and processes it. This ensures that no messages are lost, even if they cannot be delivered immediately.
- SQS provides **durability and persistence**, making it ideal for storing messages that need to be reprocessed.

#### **Why other options are incorrect**

- **Option A (Amazon SES)**: Amazon SES is an email-sending service and is not designed to store or reprocess undelivered SNS messages.
- **Option B (AWS Lambda)**: AWS Lambda is a serverless compute service and cannot store messages. It can process messages from SNS or SQS but does not provide message persistence on its own.
- **Option D (Amazon CloudWatch alarm)**: CloudWatch alarms are used for monitoring and alerting based on metrics. They cannot store or reprocess undelivered SNS messages.

#### **Key Takeaways**

- **SNS + SQS Integration**: By using SQS as a subscriber to an SNS topic, the developer can ensure that messages are stored and available for reprocessing if they cannot be delivered immediately.
- **Message Durability**: SQS provides the necessary durability and persistence for undelivered messages, making it the best choice for this use case.

Using **Amazon SQS** as a subscriber to the SNS topic ensures that undelivered messages are preserved and can be reprocessed as needed.

</details>

## Question #12

A developer needs to deploy an application to AWS Elastic Beanstalk for a company. The application consists of a single Docker image. The company's automated continuous integration and continuous delivery (CI/CD) process builds the Docker image and pushes the image to a public Docker registry.  
How should the developer deploy the application to Elastic Beanstalk?

- A. Create a Dockerfile. Configure Elastic Beanstalk to build the application as a Docker image.
- B. Create a `docker-compose.yml` file. Use the Elastic Beanstalk CLI to deploy the application.
- C. Create a `.zip` file that contains the Docker image. Upload the `.zip` file to Elastic Beanstalk.
- D. Create a Dockerfile. Run the Elastic Beanstalk CLI `eb local run` command in the same directory.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create a `docker-compose.yml` file. Use the Elastic Beanstalk CLI to deploy the application.**

### Explanation

#### **Purpose of AWS Elastic Beanstalk**

AWS Elastic Beanstalk is a fully managed service that makes it easy to deploy and run applications in multiple languages, including Docker-based applications. It handles capacity provisioning, load balancing, scaling, and application health monitoring.

#### **Why this option is correct**

- The application consists of a **single Docker image** that is already built and pushed to a **public Docker registry**. Elastic Beanstalk supports deploying Docker applications using a `Dockerrun.aws.json` file or a `docker-compose.yml` file.
- By creating a `docker-compose.yml` file, the developer can specify the Docker image from the public registry and configure the application's deployment settings. Elastic Beanstalk will pull the image from the registry and deploy it.
- The **Elastic Beanstalk CLI** (`eb`) can be used to deploy the application by running the `eb deploy` command, which reads the `docker-compose.yml` file and provisions the necessary resources.

#### **Why other options are incorrect**

- **Option A**: Creating a Dockerfile and configuring Elastic Beanstalk to build the image is unnecessary because the Docker image is already built and available in a public registry. Elastic Beanstalk does not need to build the image.
- **Option C**: Creating a `.zip` file containing the Docker image is not a valid approach for deploying Docker images to Elastic Beanstalk. Elastic Beanstalk expects a `Dockerrun.aws.json` or `docker-compose.yml` file to reference the Docker image from a registry.
- **Option D**: Running the `eb local run` command is used for testing the application locally and does not deploy the application to Elastic Beanstalk.

#### **Key Takeaways**

- **Docker Deployment on Elastic Beanstalk**: Use a `docker-compose.yml` file or `Dockerrun.aws.json` file to specify the Docker image and deployment configuration.
- **Public Docker Registry**: Elastic Beanstalk can pull Docker images from public registries, eliminating the need to build the image during deployment.
- **Elastic Beanstalk CLI**: The `eb deploy` command simplifies the deployment process by reading the configuration file and provisioning the necessary resources.

By creating a `docker-compose.yml` file and using the Elastic Beanstalk CLI, the developer can efficiently deploy the Docker-based application to Elastic Beanstalk.

</details>

## Question #13

A company is using AWS CodeDeploy for all production deployments. A developer has an Amazon Elastic Container Service (Amazon ECS) application that uses the `CodeDeployDefault.ECSAIIAtOnce` configuration. The developer needs to update the production environment in increments of 10% until the entire production environment is updated.  
Which CodeDeploy configuration should the developer use to meet these requirements?

- A. `CodeDeployDefault.ECSCanary10Percent5Minutes`
- B. `CodeDeployDefault.ECSLinear10PercentEvery3Minutes`
- C. `CodeDeployDefault.OneAtATime`
- D. `CodeDeployDefault.LambdaCanary10Percent5Minutes`

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. `CodeDeployDefault.ECSLinear10PercentEvery3Minutes`**

### Explanation

#### **Purpose of AWS CodeDeploy**

AWS CodeDeploy is a deployment service that automates application deployments to various compute services, including Amazon ECS. It supports different deployment configurations to control how traffic is shifted during updates.

#### **Why this option is correct**

- The developer needs to update the production environment **in increments of 10%** until the entire environment is updated. This is a **linear deployment strategy**, where traffic is shifted gradually in equal increments over a specified time interval.
- The `CodeDeployDefault.ECSLinear10PercentEvery3Minutes` configuration is specifically designed for ECS deployments. It shifts **10% of traffic every 3 minutes** until the deployment is complete, meeting the requirement of updating the environment in 10% increments.

#### **Why other options are incorrect**

- **Option A (`CodeDeployDefault.ECSCanary10Percent5Minutes`)**: This configuration uses a **canary deployment strategy**, where 10% of traffic is shifted initially, and the remaining 90% is shifted after a 5-minute wait. This does not meet the requirement of updating in 10% increments.
- **Option C (`CodeDeployDefault.OneAtATime`)**: This configuration updates one task at a time, which is not equivalent to updating in 10% increments. It is more suitable for small-scale deployments or testing.
- **Option D (`CodeDeployDefault.LambdaCanary10Percent5Minutes`)**: This configuration is for **AWS Lambda deployments**, not ECS. It is not applicable to the developer's use case.

#### **Key Takeaways**

- **Linear Deployment Strategy**: Shifts traffic in equal increments over time, ensuring a gradual and controlled update process.
- **ECS-Specific Configurations**: CodeDeploy provides deployment configurations tailored for ECS, such as `ECSLinear10PercentEvery3Minutes` and `ECSCanary10Percent5Minutes`.
- **Incremental Updates**: The `ECSLinear10PercentEvery3Minutes` configuration ensures that the production environment is updated in 10% increments, minimizing risk and allowing for monitoring at each stage.

By using the `CodeDeployDefault.ECSLinear10PercentEvery3Minutes` configuration, the developer can safely and incrementally update the ECS application in production.

</details>

## Question #14

A company has point-of-sale devices across thousands of retail shops that synchronize sales transactions with a centralized system. The system includes an Amazon API Gateway API that exposes an AWS Lambda function. The Lambda function processes the transactions and stores the transactions in Amazon RDS for MySQL. The number of transactions increases rapidly during the day and is near zero at night.  
How can a developer increase the elasticity of the system MOST cost-effectively?

- A. Migrate from Amazon RDS to Amazon Aurora MySQL. Use an Aurora Auto Scaling policy to scale read replicas based on CPU consumption.
- B. Migrate from Amazon RDS to Amazon Aurora MySQL. Use an Aurora Auto Scaling policy to scale read replicas based on the number of database connections.
- C. Create an Amazon Simple Queue Service (Amazon SQS) queue. Publish transactions to the queue. Set the queue to invoke the Lambda function. Turn on enhanced fanout for the Lambda function.
- D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Publish transactions to the queue. Set the queue to invoke the Lambda function. Set the reserved concurrency of the Lambda function to be less than the number of database connections.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Create an Amazon Simple Queue Service (Amazon SQS) queue. Publish transactions to the queue. Set the queue to invoke the Lambda function. Set the reserved concurrency of the Lambda function to be less than the number of database connections.**

### Explanation

#### **Purpose of Elasticity**

Elasticity refers to the ability of a system to scale resources up or down automatically based on demand. For this system, the number of transactions varies significantly throughout the day, so the solution must handle high traffic during peak hours and reduce costs during low-traffic periods.

#### **Why this option is correct**

- **Amazon SQS**: SQS is a fully managed message queuing service that decouples the point-of-sale devices from the Lambda function. Transactions can be published to the queue, ensuring that no data is lost during peak traffic.
- **Lambda Integration**: SQS can invoke the Lambda function to process messages from the queue. Lambda automatically scales based on the number of messages in the queue, ensuring that the system can handle high transaction volumes during the day.
- **Reserved Concurrency**: Setting the reserved concurrency of the Lambda function to be less than the number of database connections ensures that the Lambda function does not overwhelm the database with too many concurrent connections. This prevents database throttling and ensures smooth operation.
- **Cost-Effectiveness**: SQS and Lambda are pay-as-you-go services, meaning the company only pays for the resources used. During low-traffic periods, costs are minimized because Lambda scales down automatically.

#### **Why other options are incorrect**

- **Option A**: Migrating to Amazon Aurora MySQL and using Auto Scaling for read replicas based on CPU consumption does not address the scalability of the Lambda function or the decoupling of the transaction processing pipeline. It also introduces unnecessary complexity and cost for this use case.
- **Option B**: Similar to Option A, scaling read replicas based on the number of database connections does not solve the problem of handling high transaction volumes at the application level. It also does not address the decoupling of the system.
- **Option C**: Enhanced fanout is a feature of **Amazon Kinesis Data Streams**, not SQS. This option is invalid because SQS does not support enhanced fanout.

#### **Key Takeaways**

- **Decoupling with SQS**: Using SQS decouples the point-of-sale devices from the Lambda function, ensuring that transactions are processed asynchronously and reliably.
- **Lambda Auto Scaling**: Lambda automatically scales to process messages from the queue, providing elasticity and cost-effectiveness.
- **Reserved Concurrency**: Ensures that the Lambda function does not overwhelm the database with too many concurrent connections.

By using **SQS** and **Lambda**, the developer can create a highly elastic and cost-effective system that scales with demand and minimizes costs during low-traffic periods.

</details>

## Question #15

A developer is implementing user authentication and authorization for a web application that is hosted on an Amazon EC2 instance. The developer needs to ensure that the user credentials are encrypted and secure when they are stored and transmitted.  
Which solution will meet these requirements?

- A. Activate web server modules for authentication and authorization on the instance. Use HTTP basic authentication for the user login.
- B. Deploy a custom authentication and authorization API over HTTP. Store the user credentials on Amazon ElastiCache for Redis.
- C. Use Amazon Cognito to configure a user pool. Use the Amazon Cognito API to authenticate and authorize the users.
- D. Create IAM users. Assign the users to different IAM groups. Use AWS Single Sign-On to authenticate and authorize each user.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Use Amazon Cognito to configure a user pool. Use the Amazon Cognito API to authenticate and authorize the users.**

### Explanation

#### **Purpose of Secure Authentication and Authorization**

The developer needs to ensure that user credentials are **encrypted and secure** both when stored and transmitted. This requires a solution that provides robust security features, such as encryption, secure storage, and secure transmission protocols.

#### **Why this option is correct**

- **Amazon Cognito**: Amazon Cognito is a fully managed service that provides **user authentication and authorization** for web and mobile applications. It supports secure storage of user credentials in **user pools**, which are encrypted at rest and in transit.
- **User Pools**: Cognito user pools allow developers to create and manage a directory of users. User credentials are securely stored and managed by AWS, eliminating the need for custom credential storage solutions.
- **Secure Transmission**: Cognito uses HTTPS for all API calls, ensuring that credentials are transmitted securely.
- **Integration**: The Cognito API can be integrated with the web application hosted on the EC2 instance to handle user authentication and authorization securely.

#### **Why other options are incorrect**

- **Option A**: Using HTTP basic authentication is **not secure**. Credentials are transmitted in plaintext (base64-encoded), making them vulnerable to interception. Additionally, storing credentials on the web server is not as secure as using a managed service like Cognito.
- **Option B**: Deploying a custom API over HTTP and storing credentials in Amazon ElastiCache for Redis is **not secure**. HTTP does not encrypt data in transit, and Redis is not designed for secure credential storage. This approach introduces significant security risks.
- **Option D**: IAM users and AWS Single Sign-On (SSO) are designed for managing access to **AWS resources**, not for authenticating users of a web application. This approach is not suitable for application-level authentication and authorization.

#### **Key Takeaways**

- **Amazon Cognito**: Provides secure user authentication and authorization with built-in encryption for credentials.
- **User Pools**: Securely store and manage user credentials, eliminating the need for custom solutions.
- **HTTPS**: Ensures secure transmission of credentials between the application and Cognito.

By using **Amazon Cognito**, the developer can ensure that user credentials are encrypted and secure both when stored and transmitted, meeting the requirements of the application.

</details>

## Question #16

A company that has multiple offices uses an Amazon DynamoDB table to store employee payroll information. Item attributes consist of employee names, office identifiers, and cumulative daily hours worked. The most frequently used query extracts a report of an alphabetical subset of employees for a specific office.  
Which design of the DynamoDB table primary key will have the MINIMUM performance impact?

- A. Partition key on the office identifier and sort key on the employee name
- B. Partition key on the employee name and sort key on the office identifier
- C. Partition key on the employee name
- D. Partition key on the office identifier

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Partition key on the office identifier and sort key on the employee name**

### Explanation

#### **Purpose of DynamoDB Primary Key Design**

The primary key in DynamoDB determines how data is distributed and accessed. A well-designed primary key ensures efficient query performance and scalability.

#### **Why this option is correct**

- **Partition Key on Office Identifier**: Using the office identifier as the partition key ensures that data for each office is stored together. This allows DynamoDB to distribute data evenly across partitions while enabling efficient queries for a specific office.
- **Sort Key on Employee Name**: Using the employee name as the sort key allows for **efficient querying of an alphabetical subset of employees** within a specific office. The sort key enables range queries, which are ideal for extracting ordered subsets of data.
- **Query Efficiency**: The most frequently used query extracts a report of an alphabetical subset of employees for a specific office. With this primary key design, the query can use the **`Query` API** with the office identifier as the partition key and a range condition on the employee name, ensuring **minimal performance impact**.

#### **Why other options are incorrect**

- **Option B**: Using the employee name as the partition key and the office identifier as the sort key would scatter data across partitions, making it inefficient to query for employees in a specific office. This design does not align with the query pattern.
- **Option C**: Using only the employee name as the partition key would scatter data across partitions and make it impossible to efficiently query for employees in a specific office. This design does not support the required query pattern.
- **Option D**: Using only the office identifier as the partition key would group data by office but would not allow for efficient alphabetical sorting of employees within an office. This design lacks a sort key, making it less efficient for the required query.

#### **Key Takeaways**

- **Partition Key**: Determines how data is distributed across partitions. Use a value that aligns with the most common query pattern (e.g., office identifier).
- **Sort Key**: Enables efficient range queries and sorting within a partition. Use a value that supports the query requirements (e.g., employee name).
- **Query API**: Use the `Query` API to retrieve items based on the partition key and sort key, ensuring optimal performance.

By designing the primary key with the **office identifier as the partition key** and the **employee name as the sort key**, the developer can achieve the **minimum performance impact** for the most frequently used query.

</details>

## Question #17

A company hosts a microservices application that uses Amazon API Gateway, AWS Lambda, Amazon Simple Queue Service (Amazon SQS), and Amazon DynamoDB. One of the Lambda functions adds messages to an SQS FIFO queue.  
When a developer checks the application logs, the developer finds a few duplicated items in a DynamoDB table. The items were inserted by another polling function that processes messages from the queue.  
What is the MOST likely cause of this issue?

- A. Write operations on the DynamoDB table are being throttled.
- B. The SQS queue delivered the message to the function more than once.
- C. API Gateway duplicated the message in the SQS queue.
- D. The polling function timeout is greater than the queue visibility timeout.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. The polling function timeout is greater than the queue visibility timeout.**

### Explanation

#### **Purpose of SQS FIFO Queues**

SQS FIFO (First-In-First-Out) queues are designed to ensure that messages are processed exactly once and in the correct order. However, duplicate items can still occur if the visibility timeout and function timeout are not properly configured.

#### **Why this option is correct**

- **Visibility Timeout**: The visibility timeout is the period during which SQS prevents other consumers from receiving and processing a message after it has been picked up by a consumer.
- **Function Timeout**: If the Lambda function processing the message takes longer than the visibility timeout, SQS assumes the message was not processed and makes it available again for another consumer. This can lead to **duplicate processing** of the same message.
- **Duplicate Items in DynamoDB**: If the Lambda function processes the same message multiple times due to visibility timeout issues, it can result in duplicate items being inserted into the DynamoDB table.

#### **Why other options are incorrect**

- **Option A**: Write throttling in DynamoDB would result in failed or delayed writes, not duplicate items. Throttling does not cause duplicates.
- **Option B**: SQS FIFO queues are designed to prevent duplicate message delivery. While rare, duplicates can occur, but this is not the most likely cause in this scenario.
- **Option C**: API Gateway does not interact directly with SQS queues in this architecture. It is not responsible for duplicating messages in the queue.

#### **Key Takeaways**

- **Visibility Timeout**: Ensure that the visibility timeout of the SQS queue is greater than the maximum processing time of the Lambda function to prevent duplicate processing.
- **Function Timeout**: Configure the Lambda function timeout to be less than the SQS visibility timeout to avoid message reprocessing.
- **Idempotency**: Design the Lambda function to be idempotent, meaning it can handle duplicate messages without causing unintended side effects (e.g., duplicate items in DynamoDB).

By ensuring that the **polling function timeout is less than the queue visibility timeout**, the developer can prevent duplicate processing and avoid duplicate items in the DynamoDB table.

</details>

## Question #18

A development team has been using a builder server that is hosted on an Amazon EC2 instance to perform builds and deployments for the last 3 months. The EC2 instance's instance profile uses an IAM role that contains the `AdministratorAccess` managed policy. The development team must replace that policy with a policy that provides only the required permissions.  
What is the FASTEST way to create a custom IAM policy for the EC2 instance to meet this requirement?

- A. Create a new IAM policy based on services that the build server deployed or updated in the last 3 months.
- B. Create a new IAM policy that includes all actions that AWS CloudTrail recorded for the IAM role in the last 3 months.
- C. Create a new permissions boundary policy that denies all access. Associate the permissions boundaries with the IAM role.
- D. Create a new IAM policy by using Amazon Athena to query an Amazon S3 bucket that contains AWS CloudTrail events that the IAM role performed in the last 3 months.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create a new IAM policy that includes all actions that AWS CloudTrail recorded for the IAM role in the last 3 months.**

### Explanation

#### **Purpose of Least Privilege**

The principle of least privilege requires granting only the permissions necessary for a task. The development team needs to replace the overly permissive `AdministratorAccess` policy with a custom policy that provides only the required permissions.

#### **Why this option is correct**

- **AWS CloudTrail**: CloudTrail records all API calls made by the IAM role, including the actions performed by the build server. This provides a detailed history of the permissions actually used by the role.
- **Fastest Way**: By analyzing CloudTrail logs, the developer can quickly identify the specific actions performed by the IAM role over the last 3 months. This allows them to create a custom IAM policy that includes only those actions, ensuring least privilege.
- **Automation**: Tools like the AWS Management Console, AWS CLI, or SDKs can be used to extract and analyze CloudTrail logs, making this process efficient.

#### **Why other options are incorrect**

- **Option A**: Creating a policy based on services deployed or updated is not precise. It does not account for specific actions performed within those services, which could lead to over-permissioning or under-permissioning.
- **Option C**: A permissions boundary policy that denies all access would prevent the build server from functioning. This approach does not help create a custom policy with the required permissions.
- **Option D**: Using Amazon Athena to query CloudTrail logs stored in S3 is a valid approach but is more complex and time-consuming than directly analyzing CloudTrail logs through the AWS Management Console or CLI.

#### **Key Takeaways**

- **CloudTrail Logs**: Provide a detailed record of API calls made by the IAM role, making it easy to identify the permissions actually used.
- **Custom IAM Policy**: Create a policy that includes only the actions recorded in CloudTrail logs to ensure least privilege.
- **Efficiency**: Analyzing CloudTrail logs is the fastest and most accurate way to create a custom IAM policy tailored to the build server's needs.

By using **CloudTrail logs** to identify the actions performed by the IAM role, the developer can quickly create a custom IAM policy that meets the principle of least privilege.

</details>

## Question #19

A developer needs to write an AWS CloudFormation template on a local machine and deploy a CloudFormation stack to AWS.  
What must the developer do to complete these tasks?

- A. Install the AWS CLI. Configure the AWS CLI by using an IAM user name and password.
- B. Install the AWS CLI. Configure the AWS CLI by using an SSH key.
- C. Install the AWS CLI. Configure the AWS CLI by using an IAM user access key and secret key.
- D. Install an AWS software development kit (SDK). Configure the SDK by using an X.509 certificate.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Install the AWS CLI. Configure the AWS CLI by using an IAM user access key and secret key.**

### Explanation

#### **Purpose of AWS CLI**

The AWS Command Line Interface (CLI) is a unified tool to manage AWS services from the command line. It allows developers to interact with AWS services, including CloudFormation, directly from their local machine.

#### **Why this option is correct**

- **Install the AWS CLI**: The developer must install the AWS CLI on their local machine to deploy CloudFormation stacks. The CLI provides commands to create, update, and delete CloudFormation stacks.
- **Configure the AWS CLI**: To authenticate and authorize API calls, the developer must configure the AWS CLI with valid credentials. This is done using an **IAM user access key and secret key**, which are generated from the AWS Management Console.
- **Deploy CloudFormation Stacks**: Once the CLI is installed and configured, the developer can use the `aws cloudformation deploy` command to deploy the CloudFormation template to AWS.

#### **Why other options are incorrect**

- **Option A**: The AWS CLI cannot be configured using an IAM user name and password. It requires an **access key and secret key** for authentication.
- **Option B**: SSH keys are used for secure access to EC2 instances, not for configuring the AWS CLI.
- **Option D**: While AWS SDKs can be used to interact with AWS services, they are not required for deploying CloudFormation stacks. Additionally, X.509 certificates are not used for CLI configuration.

#### **Key Takeaways**

- **AWS CLI**: The primary tool for deploying CloudFormation stacks from a local machine.
- **Access Key and Secret Key**: Required for configuring the AWS CLI and authenticating API calls.
- **CloudFormation Commands**: Use the `aws cloudformation deploy` command to deploy stacks.

By **installing the AWS CLI** and **configuring it with an IAM user access key and secret key**, the developer can successfully deploy CloudFormation stacks to AWS.

</details>

## Question #20

A developer needs to use Amazon DynamoDB to store customer orders. The developer's company requires all customer data to be encrypted at rest with a key that the company generates.  
What should the developer do to meet these requirements?

- A. Create the DynamoDB table with encryption set to None. Code the application to use the key to decrypt the data when the application reads from the table. Code the application to use the key to encrypt the data when the application writes to the table.
- B. Store the key by using AWS Key Management Service (AWS KMS). Choose an AWS KMS customer managed key during creation of the DynamoDB table. Provide the Amazon Resource Name (ARN) of the AWS KMS key.
- C. Store the key by using AWS Key Management Service (AWS KMS). Create the DynamoDB table with default encryption. Include the `kms:Encrypt` parameter with the Amazon Resource Name (ARN) of the AWS KMS key when using the DynamoDB software development kit (SDK).
- D. Store the key by using AWS Key Management Service (AWS KMS). Choose an AWS KMS AWS managed key during creation of the DynamoDB table. Provide the Amazon Resource Name (ARN) of the AWS KMS key.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Store the key by using AWS Key Management Service (AWS KMS). Choose an AWS KMS customer managed key during creation of the DynamoDB table. Provide the Amazon Resource Name (ARN) of the AWS KMS key.**

### Explanation

#### **Purpose of Encryption at Rest**

Encryption at rest ensures that data is securely stored and protected from unauthorized access. The company requires that the encryption key be generated and managed by them, not AWS.

#### **Why this option is correct**

- **AWS KMS**: AWS Key Management Service (KMS) is a managed service that makes it easy to create and control encryption keys. It integrates with DynamoDB to provide encryption at rest.
- **Customer Managed Key**: A customer managed key is a KMS key that is created and managed by the customer, not AWS. This meets the requirement that the company generates and controls the encryption key.
- **DynamoDB Encryption**: During the creation of the DynamoDB table, the developer can specify encryption at rest using a customer managed key by providing the ARN of the KMS key. DynamoDB will automatically encrypt all data at rest using this key.

#### **Why other options are incorrect**

- **Option A**: Manually encrypting and decrypting data in the application is error-prone and inefficient. DynamoDB provides built-in encryption at rest, which is more secure and easier to manage.
- **Option C**: Default encryption in DynamoDB uses an AWS managed key, not a customer managed key. Additionally, the `kms:Encrypt` parameter is not used in this context.
- **Option D**: An AWS managed key is generated and managed by AWS, not the company. This does not meet the requirement that the company generates and controls the encryption key.

#### **Key Takeaways**

- **AWS KMS**: Use AWS KMS to create and manage encryption keys.
- **Customer Managed Key**: Choose a customer managed key to ensure that the company controls the encryption key.
- **DynamoDB Encryption**: Specify the ARN of the KMS key during table creation to enable encryption at rest.

By using **AWS KMS** with a **customer managed key**, the developer can ensure that all customer data in DynamoDB is encrypted at rest with a key generated and controlled by the company.

</details>

## Question #21

A developer is writing an AWS Lambda function. The developer wants to log key events that occur during the Lambda function and include a unique identifier to associate the events with a specific function invocation.  
Which of the following will help the developer accomplish this objective?

- A. Obtain the request identifier from the Lambda context object. Architect the application to write logs to the console.
- B. Obtain the request identifier from the Lambda event object. Architect the application to write logs to a file.
- C. Obtain the request identifier from the Lambda event object. Architect the application to write logs to the console.
- D. Obtain the request identifier from the Lambda context object. Architect the application to write logs to a file.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Obtain the request identifier from the Lambda context object. Architect the application to write logs to the console.**

### Explanation

#### **Purpose of Logging in Lambda**

Logging is essential for debugging and monitoring Lambda function invocations. Including a unique identifier in the logs helps correlate events with specific invocations.

#### **Why this option is correct**

- **Lambda Context Object**: The Lambda context object contains metadata about the invocation, including the **request ID** (`awsRequestId`). This is a unique identifier for each invocation.
- **Writing Logs to the Console**: Lambda automatically captures logs written to the console (e.g., using `console.log` in Node.js or `print` in Python) and sends them to **Amazon CloudWatch Logs**. This is the recommended way to log events in Lambda functions.
- **Unique Identifier**: By including the `awsRequestId` from the context object in the logs, the developer can associate log entries with specific invocations.

#### **Why other options are incorrect**

- **Option B**: The Lambda event object contains the input data passed to the function, not the request ID. Writing logs to a file is not recommended because Lambda functions are stateless, and files are not persisted across invocations.
- **Option C**: The event object does not contain the request ID. Writing logs to the console is correct, but the unique identifier must come from the context object.
- **Option D**: Writing logs to a file is not recommended for Lambda functions because files are not persisted across invocations. The context object is the correct source for the request ID.

#### **Key Takeaways**

- **Context Object**: Use the `awsRequestId` from the Lambda context object to uniquely identify invocations.
- **Console Logging**: Write logs to the console, as Lambda automatically sends them to CloudWatch Logs.
- **CloudWatch Logs**: Logs written to the console are stored in CloudWatch Logs, where they can be searched and analyzed.

By obtaining the **request ID from the context object** and writing logs to the **console**, the developer can effectively log key events and associate them with specific function invocations.

</details>

## Question #22

A company experienced partial downtime during the last deployment of a new application. AWS Elastic Beanstalk split the environment's Amazon EC2 instances into batches and deployed a new version one batch at a time after taking them out of service. Therefore, full capacity was not maintained during deployment.  
The developer plans to release a new version of the application, and is looking for a policy that will maintain full capacity and minimize the impact of the failed deployment.  
Which deployment policy should the developer use?

- A. Immutable
- B. All at Once
- C. Rolling
- D. Rolling with an Additional Batch

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Immutable**

### Explanation

#### **Purpose of Deployment Policies**

Deployment policies in AWS Elastic Beanstalk determine how application updates are rolled out to EC2 instances. The goal is to minimize downtime and ensure high availability during deployments.

#### **Why this option is correct**

- **Immutable Deployment**: In an immutable deployment, Elastic Beanstalk creates a **new set of instances** with the updated application version while keeping the old instances running. Once the new instances pass health checks, traffic is shifted to them, and the old instances are terminated.
- **Full Capacity**: Immutable deployments maintain full capacity throughout the deployment process because the old instances remain in service until the new instances are ready.
- **Minimized Impact**: If the deployment fails, the new instances are terminated, and the old instances continue serving traffic. This ensures minimal impact on the application's availability.

#### **Why other options are incorrect**

- **Option B: All at Once**: This policy deploys the new version to all instances simultaneously, causing downtime because all instances are taken out of service during the update.
- **Option C: Rolling**: This policy updates instances in batches, taking them out of service during the update. It does not maintain full capacity and can result in partial downtime.
- **Option D: Rolling with an Additional Batch**: This policy adds a temporary batch of instances during the deployment but still takes instances out of service during the update. It does not guarantee full capacity.

#### **Key Takeaways**

- **Immutable Deployment**: Ensures full capacity and minimal downtime by creating new instances and shifting traffic only after they pass health checks.
- **Failure Handling**: If the deployment fails, the new instances are discarded, and the old instances continue serving traffic.
- **High Availability**: Immutable deployments are ideal for critical applications that require high availability during updates.

By using the **Immutable** deployment policy, the developer can maintain full capacity and minimize the impact of a failed deployment.

</details>

## Question #23

When a Developer tries to run an AWS CodeBuild project, it raises an error because the length of all environment variables exceeds the limit for the combined maximum of characters.  
What is the recommended solution?

- A. Add the `export LC_ALL="en_US.utf8"` command to the `pre_build` section to ensure POSIX localization.
- B. Use Amazon Cognito to store key-value pairs for large numbers of environment variables.
- C. Update the settings for the build project to use an Amazon S3 bucket for large numbers of environment variables.
- D. Use AWS Systems Manager Parameter Store to store large numbers of environment variables.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Use AWS Systems Manager Parameter Store to store large numbers of environment variables.**

### Explanation

#### **Purpose of Environment Variables in CodeBuild**

Environment variables in AWS CodeBuild are used to pass configuration data to the build process. However, there is a limit on the combined size of all environment variables (currently **4 KB** for plaintext variables and **10 KB** for encrypted variables).

#### **Why this option is correct**

- **AWS Systems Manager Parameter Store**: Parameter Store is a secure, scalable, and fully managed service for storing configuration data, including environment variables. It supports storing large amounts of data and retrieving it during the build process.
- **Integration with CodeBuild**: CodeBuild can retrieve parameters from Parameter Store during the build process using the `parameter-store` option in the `env` section of the `buildspec.yml` file. This allows the developer to store large numbers of environment variables without exceeding the size limit.
- **Secure and Scalable**: Parameter Store supports encryption and versioning, making it a secure and scalable solution for managing environment variables.

#### **Why other options are incorrect**

- **Option A**: The `export LC_ALL="en_US.utf8"` command sets the locale for the build environment but does not address the issue of exceeding the environment variable size limit.
- **Option B**: Amazon Cognito is used for user authentication and identity management, not for storing environment variables or configuration data.
- **Option C**: Amazon S3 is not designed for storing environment variables. While it can store files, it is not integrated with CodeBuild for retrieving environment variables during the build process.

#### **Key Takeaways**

- **Parameter Store**: Use AWS Systems Manager Parameter Store to store large numbers of environment variables securely and scalably.
- **CodeBuild Integration**: Retrieve parameters from Parameter Store during the build process using the `parameter-store` option in the `buildspec.yml` file.
- **Size Limits**: Avoid exceeding the environment variable size limit by offloading large amounts of data to Parameter Store.

By using **AWS Systems Manager Parameter Store**, the developer can store and retrieve large numbers of environment variables without exceeding the size limit in AWS CodeBuild.

</details>

## Question #24

A Development team decides to adopt a continuous integration/continuous delivery (CI/CD) process using AWS CodePipeline and AWS CodeCommit for a new application. However, management wants a person to review and approve the code before it is deployed to production.  
How can the Development team add a manual approver to the CI/CD pipeline?

- A. Use AWS SES to send an email to approvers when their action is required. Develop a simple application that allows approvers to accept or reject a build. Invoke an AWS Lambda function to advance the pipeline when a build is accepted.
- B. If approved, add an approved tag when pushing changes to the CodeCommit repository. CodePipeline will proceed to build and deploy approved commits without interruption.
- C. Add an approval step to CodeCommit. Commits will not be saved until approved.
- D. Add an approval action to the pipeline. Configure the approval action to publish to an Amazon SNS topic when approval is required. The pipeline execution will stop and wait for an approval.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Add an approval action to the pipeline. Configure the approval action to publish to an Amazon SNS topic when approval is required. The pipeline execution will stop and wait for an approval.**

### Explanation

#### **Purpose of Manual Approval**

Manual approval steps in a CI/CD pipeline ensure that code changes are reviewed and approved by a designated person before being deployed to production. This adds a layer of control and reduces the risk of deploying faulty code.

#### **Why this option is correct**

- **Approval Action in CodePipeline**: AWS CodePipeline supports adding a **manual approval action** to the pipeline. This action pauses the pipeline and waits for a manual approval before proceeding to the next stage.
- **Amazon SNS Integration**: The approval action can be configured to send a notification to an Amazon SNS topic when approval is required. This notifies the approver via email or other subscribed endpoints.
- **Pipeline Execution**: The pipeline execution stops at the approval step and resumes only after the approver manually approves or rejects the changes. This ensures that no code is deployed to production without approval.

#### **Why other options are incorrect**

- **Option A**: While this approach could work, it is overly complex and requires custom development. CodePipeline already provides built-in support for manual approvals, making this option unnecessary.
- **Option B**: Adding an approved tag to commits does not integrate with CodePipeline's workflow. CodePipeline does not automatically recognize tags as approval mechanisms.
- **Option C**: CodeCommit does not support an approval step for commits. Commits are saved immediately upon being pushed to the repository.

#### **Key Takeaways**

- **Approval Action**: Use the built-in manual approval action in CodePipeline to pause the pipeline and require human intervention.
- **SNS Notifications**: Configure the approval action to send notifications via Amazon SNS, ensuring that approvers are notified when their action is required.
- **Pipeline Control**: Manual approval steps provide control over deployments, ensuring that only reviewed and approved code is deployed to production.

By adding an **approval action** to the pipeline and configuring it to use **Amazon SNS**, the Development team can implement a manual approval process that meets management's requirements.

</details>

## Question #25

A developer is creating a script to automate the deployment process for a serverless application. The developer wants to use an existing AWS Serverless Application Model (AWS SAM) template for the application.  
What should the developer use for the project? (Choose two.)

- A. Call `aws cloudformation package` to create the deployment package. Call `aws cloudformation deploy` to deploy the package afterward.
- B. Call `sam package` to create the deployment package. Call `sam deploy` to deploy the package afterward.
- C. Call `aws s3 cp` to upload the AWS SAM template to Amazon S3. Call `aws lambda update-function-code` to create the application.
- D. Create a ZIP package locally and call `aws serverlessrepo create-application` to create the application.
- E. Create a ZIP package and upload it to Amazon S3. Call `aws cloudformation create-stack` to create the application.

<details>
<summary>Answer</summary>
<br>

The correct answers are:

**A. Call `aws cloudformation package` to create the deployment package. Call `aws cloudformation deploy` to deploy the package afterward.**  
**B. Call `sam package` to create the deployment package. Call `sam deploy` to deploy the package afterward.**

### Explanation

#### **Purpose of AWS SAM**

AWS SAM is a framework for building serverless applications using CloudFormation. It simplifies the process of defining and deploying serverless resources like Lambda functions, APIs, and DynamoDB tables.

#### **Why these options are correct**

- **Option A: `aws cloudformation package` and `aws cloudformation deploy`**:

  - **`aws cloudformation package`**: Packages the application code and dependencies, uploads them to an S3 bucket, and generates a transformed CloudFormation template.
  - **`aws cloudformation deploy`**: Deploys the packaged application using the transformed CloudFormation template.
  - This approach is valid and leverages AWS CLI commands to automate the deployment process.

- **Option B: `sam package` and `sam deploy`**:
  - **`sam package`**: Similar to `aws cloudformation package`, it packages the application code and dependencies, uploads them to an S3 bucket, and generates a transformed CloudFormation template.
  - **`sam deploy`**: Deploys the packaged application using the transformed CloudFormation template.
  - This approach uses the AWS SAM CLI, which is specifically designed for serverless applications and provides a more streamlined experience.

#### **Why other options are incorrect**

- **Option C**: Uploading the SAM template to S3 and using `aws lambda update-function-code` does not deploy the entire serverless application. It only updates the code for a single Lambda function, ignoring other resources defined in the SAM template.
- **Option D**: Creating a ZIP package and using `aws serverlessrepo create-application` is not suitable for deploying a serverless application. The Serverless Application Repository is for sharing applications, not for deployment.
- **Option E**: Creating a ZIP package and using `aws cloudformation create-stack` does not leverage the SAM template effectively. It requires manual steps and does not handle packaging and deployment as seamlessly as the SAM CLI or CloudFormation commands.

#### **Key Takeaways**

- **AWS SAM CLI**: Use `sam package` and `sam deploy` for a streamlined serverless deployment process.
- **AWS CloudFormation Commands**: Use `aws cloudformation package` and `aws cloudformation deploy` as an alternative to the SAM CLI.
- **Automation**: Both approaches automate the packaging and deployment of serverless applications, ensuring consistency and efficiency.

By using either **AWS SAM CLI** or **AWS CloudFormation commands**, the developer can automate the deployment process for the serverless application effectively.

</details>

## Question #26

The developer is creating a web application that collects highly regulated and confidential user data through a POST request. The web application is served through Amazon CloudFront. User names and phone numbers must be encrypted at the edge and must remain encrypted throughout the entire application stack.  
What is the MOST secure way to meet these requirements?

- A. Enforce Match Viewer with HTTPS Only on CloudFront.
- B. Use only the newest TLS security policy on CloudFront.
- C. Enforce a signed URL on CloudFront on the front end.
- D. Use field-level encryption on CloudFront.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Use field-level encryption on CloudFront.**

### Explanation

#### **Purpose of Encryption for Confidential Data**

To protect highly regulated and confidential user data, such as names and phone numbers, the data must be encrypted at the edge (when it enters CloudFront) and remain encrypted throughout the application stack.

#### **Why this option is correct**

- **Field-Level Encryption**: CloudFront's field-level encryption allows specific fields (e.g., user names and phone numbers) in HTTPS requests to be encrypted at the edge using public key cryptography. The encrypted data remains encrypted as it travels through the application stack and can only be decrypted by the intended recipient (e.g., the origin server) using the corresponding private key.
- **End-to-End Encryption**: Field-level encryption ensures that sensitive data is encrypted at the edge and remains encrypted until it reaches the origin server, providing end-to-end security.
- **Compliance**: This approach meets the requirement of encrypting sensitive data at the edge and maintaining encryption throughout the application stack.

#### **Why other options are incorrect**

- **Option A**: Enforcing HTTPS with "Match Viewer" ensures that CloudFront uses HTTPS for communication with viewers and the origin. However, it does not encrypt specific fields in the request payload.
- **Option B**: Using the newest TLS security policy ensures that CloudFront uses the latest encryption protocols, but it does not provide field-level encryption for sensitive data.
- **Option C**: Enforcing signed URLs ensures that only authorized users can access CloudFront content, but it does not encrypt specific fields in the request payload.

#### **Key Takeaways**

- **Field-Level Encryption**: Encrypts specific fields in HTTPS requests at the edge, ensuring end-to-end encryption for sensitive data.
- **Public Key Cryptography**: Uses public and private keys to encrypt and decrypt data, ensuring that only the intended recipient can access the sensitive information.
- **Compliance**: Meets regulatory requirements for encrypting sensitive data at the edge and throughout the application stack.

By using **field-level encryption on CloudFront**, the developer can ensure that user names and phone numbers are encrypted at the edge and remain encrypted throughout the application stack, providing the highest level of security.

</details>

## Question #27

A Developer has been asked to create an AWS Lambda function that is triggered any time updates are made to items in an Amazon DynamoDB table. The function has been created, and appropriate permissions have been added to the Lambda execution role. Amazon DynamoDB streams have been enabled for the table, but the function is still not being triggered.  
Which option would enable DynamoDB table updates to trigger the Lambda function?

- A. Change the `StreamViewType` parameter value to `NEW_AND_OLD_IMAGES` for the DynamoDB table
- B. Configure event source mapping for the Lambda function
- C. Map an Amazon SNS topic to the DynamoDB streams
- D. Increase the maximum execution time (timeout) setting of the Lambda function

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Configure event source mapping for the Lambda function**

### Explanation

#### **Purpose of DynamoDB Streams and Lambda Integration**

DynamoDB Streams capture changes to items in a DynamoDB table, and Lambda can be triggered to process these changes in real time. However, for this integration to work, an **event source mapping** must be configured.

#### **Why this option is correct**

- **Event Source Mapping**: Event source mapping is the mechanism that connects a DynamoDB stream to a Lambda function. It tells Lambda to poll the stream for changes and invoke the function when updates occur.
- **How to Configure**: The developer can configure event source mapping using the AWS Management Console, AWS CLI, or SDKs. This involves specifying the DynamoDB stream ARN and the Lambda function ARN.
- **Permissions**: Ensure that the Lambda execution role has the necessary permissions to read from the DynamoDB stream (`dynamodb:GetRecords`, `dynamodb:GetShardIterator`, and `dynamodb:DescribeStream`).

#### **Why other options are incorrect**

- **Option A**: Changing the `StreamViewType` parameter to `NEW_AND_OLD_IMAGES` determines what data is included in the stream (e.g., old and new images of the item). However, this does not enable the Lambda function to be triggered by the stream.
- **Option C**: Mapping an Amazon SNS topic to DynamoDB streams is not a valid configuration. SNS is used for pub/sub messaging, not for triggering Lambda functions from DynamoDB streams.
- **Option D**: Increasing the Lambda function's timeout setting does not address the issue of triggering the function. The function is not being invoked because the event source mapping is missing.

#### **Key Takeaways**

- **Event Source Mapping**: Required to connect a DynamoDB stream to a Lambda function.
- **StreamViewType**: Determines the data included in the stream but does not enable triggering.
- **Permissions**: Ensure the Lambda execution role has the necessary permissions to read from the DynamoDB stream.

By **configuring event source mapping**, the developer can enable DynamoDB table updates to trigger the Lambda function.

</details>

## Question #28

A company maintains a REST service using Amazon API Gateway and the API Gateway native API key validation. The company recently launched a new registration page, which allows users to sign up for the service. The registration page creates a new API key using `CreateApiKey` and sends the new key to the user. When the user attempts to call the API using this key, the user receives a `403 Forbidden` error. Existing users are unaffected and can still call the API.  
What code updates will grant these new users access to the API?

- A. The `createDeployment` method must be called so the API can be redeployed to include the newly created API key.
- B. The `updateAuthorizer` method must be called to update the API's authorizer to include the newly created API key.
- C. The `importApiKeys` method must be called to import all newly created API keys into the current stage of the API.
- D. The `createUsagePlanKey` method must be called to associate the newly created API key with the correct usage plan.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. The `createUsagePlanKey` method must be called to associate the newly created API key with the correct usage plan.**

### Explanation

#### **Purpose of API Keys and Usage Plans in API Gateway**

API keys are used to identify and control access to APIs in API Gateway. Usage plans define who can access specific API stages and methods, as well as rate limits and quotas.

#### **Why this option is correct**

- **API Key Creation**: The `CreateApiKey` method creates a new API key, but the key is not automatically associated with a usage plan.
- **Usage Plan Association**: To grant access to the API, the new API key must be associated with a usage plan using the `createUsagePlanKey` method. This ensures that the key is authorized to access the API and enforces any rate limits or quotas defined in the usage plan.
- **403 Forbidden Error**: The `403 Forbidden` error occurs because the new API key is not associated with a usage plan, so API Gateway denies access.

#### **Why other options are incorrect**

- **Option A**: The `createDeployment` method is used to deploy an API to a stage. It does not associate API keys with usage plans.
- **Option B**: The `updateAuthorizer` method is used to update a custom authorizer, not to associate API keys with usage plans.
- **Option C**: The `importApiKeys` method is used to import API keys from an external source, not to associate them with usage plans.

#### **Key Takeaways**

- **API Key Creation**: Use `CreateApiKey` to generate new API keys.
- **Usage Plan Association**: Use `createUsagePlanKey` to associate API keys with usage plans and grant access to the API.
- **403 Forbidden Error**: This error typically indicates that the API key is not associated with a usage plan or does not have the necessary permissions.

By calling the **`createUsagePlanKey` method**, the developer can associate the newly created API key with the correct usage plan, granting new users access to the API.

</details>

## Question #29

A company is running a Docker application on Amazon ECS. The application must scale based on user load in the last 15 seconds.  
How should a Developer instrument the code so that the requirement can be met?

- A. Create a high-resolution custom Amazon CloudWatch metric for user activity data, then publish data every 30 seconds
- B. Create a high-resolution custom Amazon CloudWatch metric for user activity data, then publish data every 5 seconds
- C. Create a standard-resolution custom Amazon CloudWatch metric for user activity data, then publish data every 30 seconds
- D. Create a standard-resolution custom Amazon CloudWatch metric for user activity data, then publish data every 5 seconds

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Create a high-resolution custom Amazon CloudWatch metric for user activity data, then publish data every 5 seconds**

### Explanation

#### **Purpose of Scaling Based on User Load**

To scale the application dynamically based on user load, the developer needs to collect and analyze user activity data in near real time. This requires high-resolution metrics and frequent data publishing.

#### **Why this option is correct**

- **High-Resolution Custom Metric**: High-resolution metrics in CloudWatch support a granularity of **1 second**, which is necessary for scaling decisions based on user load in the last 15 seconds.
- **Publishing Data Every 5 Seconds**: Publishing data every 5 seconds ensures that the metric reflects recent user activity and provides sufficient data points for accurate scaling decisions.
- **Real-Time Scaling**: High-resolution metrics and frequent data publishing enable the application to scale quickly and accurately in response to changes in user load.

#### **Why other options are incorrect**

- **Option A**: Publishing data every 30 seconds is too infrequent for scaling based on user load in the last 15 seconds. This would result in delayed or inaccurate scaling decisions.
- **Option C**: Standard-resolution metrics support a granularity of **1 minute**, which is insufficient for scaling based on user load in the last 15 seconds.
- **Option D**: While publishing data every 5 seconds is frequent enough, standard-resolution metrics do not support the required granularity for real-time scaling.

#### **Key Takeaways**

- **High-Resolution Metrics**: Use high-resolution custom metrics for granularity of 1 second.
- **Frequent Data Publishing**: Publish data every 5 seconds to ensure accurate scaling decisions.
- **Real-Time Scaling**: High-resolution metrics and frequent data publishing enable real-time scaling based on user load.

By creating a **high-resolution custom CloudWatch metric** and publishing data **every 5 seconds**, the developer can meet the requirement of scaling the application based on user load in the last 15 seconds.

</details>

## Question #30

A Developer is working on an application that handles 10MB documents that contain highly-sensitive data. The application will use AWS KMS to perform client-side encryption.  
What steps must be followed?

- A. Invoke the `Encrypt` API passing the plaintext data that must be encrypted, then reference the customer managed key ARN in the `KeyId` parameter
- B. Invoke the `GenerateRandom` API to get a data encryption key, then use the data encryption key to encrypt the data
- C. Invoke the `GenerateDataKey` API to retrieve the encrypted version of the data encryption key to encrypt the data
- D. Invoke the `GenerateDataKey` API to retrieve the plaintext version of the data encryption key to encrypt the data

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Invoke the `GenerateDataKey` API to retrieve the plaintext version of the data encryption key to encrypt the data**

### Explanation

#### **Purpose of Client-Side Encryption**

Client-side encryption ensures that sensitive data is encrypted before it is sent to AWS. This provides an additional layer of security, as the data is never transmitted or stored in plaintext.

#### **Why this option is correct**

- **GenerateDataKey API**: The `GenerateDataKey` API generates a **data encryption key (DEK)** that can be used to encrypt data. It returns both a plaintext version and an encrypted version of the DEK.
- **Plaintext DEK**: The plaintext version of the DEK is used to encrypt the data locally on the client side. After encryption, the plaintext DEK should be securely erased from memory.
- **Encrypted DEK**: The encrypted version of the DEK is stored alongside the encrypted data. It can be decrypted later using AWS KMS to retrieve the plaintext DEK for decryption.
- **Efficiency**: Using a DEK for client-side encryption is more efficient than encrypting large data directly with KMS, as KMS has a limit of 4 KB for the `Encrypt` API.

#### **Why other options are incorrect**

- **Option A**: The `Encrypt` API is limited to 4 KB of data and is not suitable for encrypting 10MB documents. It is also less efficient for large data.
- **Option B**: The `GenerateRandom` API generates random data but does not provide a data encryption key for encrypting sensitive data.
- **Option C**: Retrieving only the encrypted version of the DEK does not allow the client to encrypt the data locally. The plaintext DEK is required for client-side encryption.

#### **Key Takeaways**

- **GenerateDataKey API**: Use this API to generate a data encryption key for client-side encryption.
- **Plaintext DEK**: Use the plaintext DEK to encrypt data locally, then securely erase it.
- **Encrypted DEK**: Store the encrypted DEK alongside the encrypted data for future decryption.

By invoking the **`GenerateDataKey` API** and using the **plaintext version of the DEK**, the developer can perform client-side encryption of the 10MB documents securely and efficiently.

</details>

## Question #31

An application uses Amazon Kinesis Data Streams to ingest and process large streams of data records in real time. Amazon EC2 instances consume and process the data from the shards of the Kinesis data stream by using Amazon Kinesis Client Library (KCL). The application handles the failure scenarios and does not require standby workers. The application reports that a specific shard is receiving more data than expected. To adapt to the changes in the rate of data flow, the `hot` shard is resharded.  
Assuming that the initial number of shards in the Kinesis data stream is 4, and after resharding the number of shards increased to 6, what is the maximum number of EC2 instances that can be deployed to process data from all the shards?

- A. 12
- B. 6
- C. 4
- D. 1

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. 6**

### Explanation

#### **Purpose of Shards in Kinesis Data Streams**

Shards are the base throughput units of a Kinesis data stream. Each shard can support up to **1 MB/s of data input and 2 MB/s of data output**. Each shard can be processed by **one consumer** at a time.

#### **Why this option is correct**

- **One Consumer per Shard**: Each shard can be processed by only one EC2 instance at a time. Therefore, the maximum number of EC2 instances that can be deployed is equal to the number of shards.
- **Resharding**: After resharding, the number of shards increased from 4 to 6. This means that up to **6 EC2 instances** can be deployed to process data from all the shards.
- **No Standby Workers**: Since the application does not require standby workers, the maximum number of EC2 instances is determined solely by the number of shards.

#### **Why other options are incorrect**

- **Option A (12)**: This exceeds the number of shards (6) and is not possible, as each shard can only be processed by one EC2 instance.
- **Option C (4)**: This reflects the initial number of shards before resharding. After resharding, the number of shards increased to 6, allowing for more EC2 instances.
- **Option D (1)**: This is insufficient to process data from all 6 shards, as each shard requires its own consumer.

#### **Key Takeaways**

- **Shard-to-Instance Ratio**: Each shard can be processed by one EC2 instance at a time.
- **Resharding Impact**: Resharding increases the number of shards, allowing for more EC2 instances to process data in parallel.
- **Maximum Instances**: The maximum number of EC2 instances is equal to the number of shards.

By deploying **6 EC2 instances**, the developer can ensure that all 6 shards are processed efficiently.

</details>

## Question #32

A Company runs continuous integration/continuous delivery (CI/CD) pipelines for its application on AWS CodePipeline. A Developer must write unit tests and run them as part of the pipelines before staging the artifacts for testing.  
How should the Developer incorporate unit tests as part of CI/CD pipelines?

- A. Create a separate CodePipeline pipeline to run unit tests
- B. Update the AWS CodeBuild specification to include a phase for running unit tests
- C. Install the AWS CodeDeploy agent on an Amazon EC2 instance to run unit tests
- D. Create a testing branch in AWS CodeCommit to run unit tests

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Update the AWS CodeBuild specification to include a phase for running unit tests**

### Explanation

#### **Purpose of Unit Tests in CI/CD Pipelines**

Unit tests are essential for ensuring code quality and catching issues early in the development process. They should be integrated into the CI/CD pipeline to automatically validate code changes before deployment.

#### **Why this option is correct**

- **AWS CodeBuild**: CodeBuild is a fully managed build service that compiles source code, runs tests, and produces artifacts. It is integrated with CodePipeline and can be used to run unit tests as part of the build process.
- **Buildspec File**: The developer can update the `buildspec.yml` file (CodeBuild's build specification) to include a **test phase**. This phase can execute unit tests and fail the build if any tests fail, ensuring that only tested code progresses to the next stage of the pipeline.
- **Seamless Integration**: By incorporating unit tests into the CodeBuild phase, the developer ensures that tests are run automatically as part of the pipeline without requiring additional infrastructure or manual intervention.

#### **Why other options are incorrect**

- **Option A**: Creating a separate pipeline for unit tests adds unnecessary complexity and overhead. Unit tests should be integrated into the existing pipeline to ensure they are run automatically for every code change.
- **Option C**: The CodeDeploy agent is used for deploying applications to EC2 instances, not for running unit tests. This approach is not suitable for integrating unit tests into the CI/CD pipeline.
- **Option D**: Creating a testing branch in CodeCommit does not automate the execution of unit tests. Tests must still be run manually or through a separate process, which defeats the purpose of CI/CD automation.

#### **Key Takeaways**

- **CodeBuild Integration**: Use CodeBuild to run unit tests as part of the build process.
- **Buildspec File**: Update the `buildspec.yml` file to include a test phase for running unit tests.
- **Automated Testing**: Ensure that unit tests are automatically executed for every code change, improving code quality and catching issues early.

By updating the **AWS CodeBuild specification** to include a phase for running unit tests, the developer can seamlessly integrate unit testing into the CI/CD pipeline.

</details>

## Question #33

A Development team is working on a case management solution that allows medical claims to be processed and reviewed. Users log in to provide information related to their medical and financial situations.  
As part of the application, sensitive documents such as medical records, medical imaging, bank statements, and receipts are uploaded to Amazon S3. All documents must be securely transmitted and stored. All access to the documents must be recorded for auditing.  
What is the MOST secure approach?

- A. Use S3 default encryption using Advanced Encryption Standard-256 (AES-256) on the destination bucket.
- B. Use Amazon Cognito for authorization and authentication to ensure the security of the application and documents.
- C. Use AWS Lambda to encrypt and decrypt objects as they are placed into the S3 bucket.
- D. Use client-side encryption/decryption with Amazon S3 and AWS KMS.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. Use client-side encryption/decryption with Amazon S3 and AWS KMS.**

### Explanation

#### **Purpose of Secure Document Handling**

The application handles highly sensitive documents, so it is critical to ensure that the documents are securely transmitted, stored, and accessed. Additionally, all access must be recorded for auditing purposes.

#### **Why this option is correct**

- **Client-Side Encryption**: Encrypting documents on the client side before uploading them to S3 ensures that the data is never transmitted or stored in plaintext. This provides the highest level of security.
- **AWS KMS Integration**: AWS Key Management Service (KMS) can be used to manage encryption keys for client-side encryption. KMS provides robust key management and auditing capabilities.
- **Auditing**: AWS CloudTrail can be used to log all access to the S3 bucket and KMS keys, ensuring compliance with auditing requirements.
- **End-to-End Security**: Client-side encryption ensures that sensitive documents are protected throughout their lifecycle, from transmission to storage.

#### **Why other options are incorrect**

- **Option A**: S3 default encryption using AES-256 encrypts data at rest but does not protect data during transmission. It also does not provide client-side encryption, which is more secure.
- **Option B**: Amazon Cognito is used for user authentication and authorization but does not provide encryption for documents stored in S3.
- **Option C**: Using AWS Lambda to encrypt and decrypt objects in S3 does not provide end-to-end security, as the data is transmitted to S3 in plaintext before encryption.

#### **Key Takeaways**

- **Client-Side Encryption**: Encrypt documents on the client side before uploading them to S3 to ensure end-to-end security.
- **AWS KMS**: Use KMS to manage encryption keys and provide auditing capabilities.
- **CloudTrail**: Enable CloudTrail to log all access to S3 and KMS for auditing purposes.

By using **client-side encryption/decryption with Amazon S3 and AWS KMS**, the Development team can ensure the highest level of security for sensitive documents and meet auditing requirements.

</details>

## Question #34

An application needs to use the IP address of the client in its processing. The application has been moved into AWS and has been placed behind an Application Load Balancer (ALB). However, all the client IP addresses now appear to be the same. The application must maintain the ability to scale horizontally.  
Based on this scenario, what is the MOST cost-effective solution to this problem?

- A. Remove the application from the ALB. Delete the ALB and change Amazon Route 53 to direct traffic to the instance running the application.
- B. Remove the application from the ALB. Create a Classic Load Balancer in its place. Direct traffic to the application using the HTTP protocol.
- C. Alter the application code to inspect the `X-Forwarded-For` header. Ensure that the code can work properly if a list of IP addresses is passed in the header.
- D. Alter the application code to inspect a custom header. Alter the client code to pass the IP address in the custom header.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Alter the application code to inspect the `X-Forwarded-For` header. Ensure that the code can work properly if a list of IP addresses is passed in the header.**

### Explanation

#### **Purpose of Client IP Address**

The application requires the client's IP address for processing. When placed behind an ALB, the client's IP address is replaced by the ALB's IP address. To retrieve the original client IP address, the application must inspect the `X-Forwarded-For` header.

#### **Why this option is correct**

- **X-Forwarded-For Header**: The ALB automatically adds the `X-Forwarded-For` header to incoming requests. This header contains the original client IP address and any intermediate proxy IP addresses.
- **Code Modification**: The application code can be updated to inspect the `X-Forwarded-For` header and extract the client IP address. This is a simple and cost-effective solution that does not require changes to the infrastructure.
- **Horizontal Scaling**: The solution maintains the ability to scale horizontally, as the ALB can distribute traffic across multiple instances of the application.

#### **Why other options are incorrect**

- **Option A**: Removing the ALB and using Route 53 to direct traffic to a single instance eliminates the ability to scale horizontally and introduces a single point of failure.
- **Option B**: Replacing the ALB with a Classic Load Balancer does not solve the issue of retrieving the client IP address. Classic Load Balancers also replace the client IP address with their own.
- **Option D**: Using a custom header requires modifying both the client and server code, which is more complex and less scalable than using the `X-Forwarded-For` header.

#### **Key Takeaways**

- **X-Forwarded-For Header**: Use this header to retrieve the original client IP address when the application is behind an ALB.
- **Code Modification**: Update the application code to inspect the `X-Forwarded-For` header and extract the client IP address.
- **Horizontal Scaling**: Maintain the ability to scale horizontally by keeping the ALB in place.

By altering the application code to inspect the **`X-Forwarded-For` header**, the developer can retrieve the client IP address while maintaining the ability to scale horizontally.

</details>

## Question #35

A Developer must analyze performance issues with production-distributed applications written as AWS Lambda functions. These distributed Lambda applications invoke other components that make up the applications.  
How should the Developer identify and troubleshoot the root cause of the performance issues in production?

- A. Add logging statements to the Lambda functions, then use Amazon CloudWatch to view the logs.
- B. Use AWS CloudTrail and then examine the logs.
- C. Use AWS X-Ray, then examine the segments and errors.
- D. Run Amazon Inspector agents and then analyze performance.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Use AWS X-Ray, then examine the segments and errors.**

### Explanation

#### **Purpose of Performance Analysis**

The developer needs to identify and troubleshoot performance issues in a distributed application composed of multiple Lambda functions and other components. This requires tracing requests across all components to pinpoint bottlenecks or errors.

#### **Why this option is correct**

- **AWS X-Ray**: X-Ray is a distributed tracing service that provides end-to-end visibility into requests as they travel through the application. It helps identify performance bottlenecks, errors, and latency issues.
- **Segments and Subsegments**: X-Ray breaks down requests into segments (for each service) and subsegments (for individual operations). This allows the developer to analyze the performance of each component and identify the root cause of issues.
- **Error Analysis**: X-Ray captures errors and exceptions, making it easier to troubleshoot issues in production.
- **Integration with Lambda**: X-Ray integrates seamlessly with AWS Lambda, automatically tracing requests and providing detailed insights into function execution.

#### **Why other options are incorrect**

- **Option A**: Adding logging statements and using CloudWatch Logs can provide insights into function execution, but it does not provide end-to-end tracing across distributed components. This approach is less efficient for identifying performance bottlenecks.
- **Option B**: AWS CloudTrail logs API calls for auditing and security purposes but does not provide performance tracing or insights into distributed applications.
- **Option D**: Amazon Inspector is a security assessment service and is not designed for performance analysis or distributed tracing.

#### **Key Takeaways**

- **AWS X-Ray**: Use X-Ray for distributed tracing and performance analysis of Lambda-based applications.
- **Segments and Errors**: Examine segments and errors in X-Ray to identify bottlenecks and troubleshoot issues.
- **Integration**: X-Ray integrates with Lambda and other AWS services, providing comprehensive visibility into distributed applications.

By using **AWS X-Ray**, the developer can effectively identify and troubleshoot the root cause of performance issues in the distributed Lambda application.

</details>

## Question #36

A company runs an e-commerce website that uses Amazon DynamoDB where pricing for items is dynamically updated in real time. At any given time, multiple updates may occur simultaneously for pricing information on a particular product. This is causing the original editor's changes to be overwritten without a proper review process.  
Which DynamoDB write option should be selected to prevent this overwriting?

- A. Concurrent writes
- B. Conditional writes
- C. Atomic writes
- D. Batch writes

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Conditional writes**

### Explanation

#### **Purpose of Preventing Overwrites**

To ensure that pricing updates are properly reviewed and not overwritten by simultaneous updates, the application needs a mechanism to enforce conditional updates based on specific criteria.

#### **Why this option is correct**

- **Conditional Writes**: Conditional writes allow you to specify conditions that must be met for a write operation to succeed. For example, you can ensure that an update only occurs if the current value of an attribute matches an expected value.
- **Preventing Overwrites**: By using conditional writes, the application can ensure that updates to pricing information are only applied if the data has not been modified since it was last read. This prevents overwriting changes made by other editors.
- **Review Process**: Conditional writes can be used to implement a review process, where updates are only applied after certain conditions (e.g., approval) are met.

#### **Why other options are incorrect**

- **Option A (Concurrent writes)**: Concurrent writes do not prevent overwrites. They allow multiple writers to update the same item simultaneously, which can lead to conflicts and overwrites.
- **Option C (Atomic writes)**: Atomic writes ensure that a write operation is completed as a single, indivisible operation. However, they do not prevent overwrites by other writers.
- **Option D (Batch writes)**: Batch writes allow you to perform multiple write operations in a single request, but they do not provide a mechanism to prevent overwrites.

#### **Key Takeaways**

- **Conditional Writes**: Use conditional writes to enforce specific conditions for updates, preventing overwrites and ensuring data integrity.
- **Review Process**: Implement a review process by using conditional writes to ensure that updates are only applied after certain conditions are met.
- **Data Integrity**: Conditional writes help maintain data integrity in scenarios where multiple updates may occur simultaneously.

By using **conditional writes**, the developer can prevent overwrites and ensure that pricing updates are properly reviewed before being applied.

</details>

## Question #37

A front-end web application is using Amazon Cognito user pools to handle the user authentication flow. A developer is integrating Amazon DynamoDB into the application using the AWS SDK for JavaScript.  
How would the developer securely call the API without exposing the access or secret keys?

- A. Configure Amazon Cognito identity pools and exchange the JSON Web Token (JWT) for temporary credentials.
- B. Run the web application in an Amazon EC2 instance with the instance profile configured.
- C. Hardcode the credentials, use Amazon S3 to host the web application, and enable server-side encryption.
- D. Use Amazon Cognito user pool JSON Web Tokens (JWTs) to access the DynamoDB APIs.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Configure Amazon Cognito identity pools and exchange the JSON Web Token (JWT) for temporary credentials.**

### Explanation

#### **Purpose of Secure API Calls**

To securely call DynamoDB APIs from a front-end web application, the developer must avoid exposing access or secret keys, which can be compromised if embedded in client-side code.

#### **Why this option is correct**

- **Amazon Cognito Identity Pools**: Identity pools allow you to exchange a JWT (from a Cognito user pool) for temporary AWS credentials. These credentials can be used to securely call AWS services like DynamoDB.
- **Temporary Credentials**: Temporary credentials are short-lived and automatically rotated, reducing the risk of exposure. They are scoped to specific permissions, ensuring least privilege.
- **Integration with AWS SDK**: The AWS SDK for JavaScript can use the temporary credentials to make secure API calls to DynamoDB without exposing access or secret keys.

#### **Why other options are incorrect**

- **Option B**: Running the web application in an EC2 instance with an instance profile is not suitable for front-end applications. Instance profiles are used for server-side applications, not client-side code.
- **Option C**: Hardcoding credentials in client-side code is highly insecure, as the credentials can be easily exposed. Server-side encryption in S3 does not address this issue.
- **Option D**: JWTs from Cognito user pools are used for authentication, not for directly accessing DynamoDB APIs. They must be exchanged for temporary credentials through an identity pool.

#### **Key Takeaways**

- **Cognito Identity Pools**: Use identity pools to exchange JWTs for temporary AWS credentials.
- **Temporary Credentials**: Temporary credentials are secure, short-lived, and scoped to specific permissions.
- **AWS SDK Integration**: Use the AWS SDK for JavaScript to make secure API calls with temporary credentials.

By configuring **Amazon Cognito identity pools** and exchanging the JWT for **temporary credentials**, the developer can securely call DynamoDB APIs without exposing access or secret keys.

</details>

## Question #38

A Developer must build an application that uses Amazon DynamoDB. The requirements state that the items being stored in the DynamoDB table will be 7KB in size and that reads must be strongly consistent. The maximum read rate is 3 items per second, and the maximum write rate is 10 items per second.  
How should the Developer size the DynamoDB table to meet these requirements?

- A. Read: 3 read capacity units Write: 70 write capacity units
- B. Read: 6 read capacity units Write: 70 write capacity units
- C. Read: 6 read capacity units Write: 10 write capacity units
- D. Read: 3 read capacity units Write: 10 write capacity units

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Read: 6 read capacity units Write: 70 write capacity units**

### Explanation

#### **Purpose of DynamoDB Capacity Units**

DynamoDB uses read capacity units (RCUs) and write capacity units (WCUs) to measure the throughput of a table. Properly sizing these units ensures that the table can handle the required read and write rates.

#### **Why this option is correct**

- **Read Capacity Units (RCUs)**:

  - Each strongly consistent read of an item up to 4 KB in size consumes **1 RCU**.
  - Since the items are 7 KB in size, each read will consume **2 RCUs** (7 KB ÷ 4 KB = 1.75, rounded up to 2).
  - The maximum read rate is 3 items per second, so the total RCUs required are **3 items/second × 2 RCUs/item = 6 RCUs**.

- **Write Capacity Units (WCUs)**:
  - Each write of an item up to 1 KB in size consumes **1 WCU**.
  - Since the items are 7 KB in size, each write will consume **7 WCUs** (7 KB ÷ 1 KB = 7).
  - The maximum write rate is 10 items per second, so the total WCUs required are **10 items/second × 7 WCUs/item = 70 WCUs**.

#### **Why other options are incorrect**

- **Option A**: This option underestimates the write capacity units (10 WCUs instead of 70).
- **Option C**: This option underestimates the write capacity units (10 WCUs instead of 70).
- **Option D**: This option underestimates both the read capacity units (3 RCUs instead of 6) and the write capacity units (10 WCUs instead of 70).

#### **Key Takeaways**

- **Strongly Consistent Reads**: Each strongly consistent read of an item up to 4 KB consumes 1 RCU. For larger items, round up to the nearest 4 KB.
- **Writes**: Each write of an item up to 1 KB consumes 1 WCU. For larger items, round up to the nearest 1 KB.
- **Throughput Calculation**: Multiply the number of items per second by the RCUs or WCUs per item to determine the total capacity units required.

By sizing the DynamoDB table with **6 RCUs** and **70 WCUs**, the developer can meet the application's read and write requirements.

</details>

## Question #39

For a deployment using AWS CodeDeploy, what is the run order of the hooks for in-place deployments?

- A. Before Install -> Application Stop -> Application Start -> After Install
- B. Application Stop -> Before Install -> After Install -> Application Start
- C. Before Install -> Application Stop -> Validate Service -> Application Start
- D. Application Stop -> Before Install -> Validate Service -> Application Start

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Application Stop -> Before Install -> After Install -> Application Start**

### Explanation

#### **Purpose of Deployment Hooks**

AWS CodeDeploy uses lifecycle event hooks to execute scripts at specific stages of the deployment process. These hooks allow you to control the deployment workflow, such as stopping the application, installing new files, and starting the application.

#### **Why this option is correct**

- **Application Stop**: The first step is to stop the application to ensure that no files are in use during the deployment.
- **Before Install**: After stopping the application, the `BeforeInstall` hook runs to perform any pre-installation tasks, such as backing up files or configuring the environment.
- **After Install**: Once the new application files are installed, the `AfterInstall` hook runs to perform post-installation tasks, such as setting file permissions or updating configurations.
- **Application Start**: Finally, the application is started with the new version.

#### **Why other options are incorrect**

- **Option A**: This option incorrectly places `Before Install` before `Application Stop`. The application must be stopped before any installation tasks begin.
- **Option C**: This option includes `Validate Service`, which is not part of the standard in-place deployment lifecycle hooks.
- **Option D**: This option also includes `Validate Service`, which is not part of the standard in-place deployment lifecycle hooks.

#### **Key Takeaways**

- **Application Stop**: Stop the application before making any changes.
- **Before Install**: Perform pre-installation tasks after stopping the application.
- **After Install**: Perform post-installation tasks after installing the new files.
- **Application Start**: Start the application with the new version.

By following the correct order of **Application Stop -> Before Install -> After Install -> Application Start**, the developer can ensure a smooth and successful deployment using AWS CodeDeploy.

</details>

## Question #40

An Amazon Kinesis Data Firehose delivery stream is receiving customer data that contains personally identifiable information. A developer needs to remove pattern-based customer identifiers from the data and store the modified data in an Amazon S3 bucket.  
What should the developer do to meet these requirements?

- A. Implement Kinesis Data Firehose data transformation as an AWS Lambda function. Configure the function to remove the customer identifiers. Set an Amazon S3 bucket as the destination of the delivery stream.
- B. Launch an Amazon EC2 instance. Set the EC2 instance as the destination of the delivery stream. Run an application on the EC2 instance to remove the customer identifiers. Store the transformed data in an Amazon S3 bucket.
- C. Create an Amazon OpenSearch Service instance. Set the OpenSearch Service instance as the destination of the delivery stream. Use search and replace to remove the customer identifiers. Export the data to an Amazon S3 bucket.
- D. Create an AWS Step Functions workflow to remove the customer identifiers. As the last step in the workflow, store the transformed data in an Amazon S3 bucket. Set the workflow as the destination of the delivery stream.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Implement Kinesis Data Firehose data transformation as an AWS Lambda function. Configure the function to remove the customer identifiers. Set an Amazon S3 bucket as the destination of the delivery stream.**

### Explanation

#### **Purpose of Data Transformation**

The developer needs to remove personally identifiable information (PII) from the data before storing it in S3. This requires transforming the data in real time as it flows through the Kinesis Data Firehose delivery stream.

#### **Why this option is correct**

- **Kinesis Data Firehose Data Transformation**: Kinesis Data Firehose supports **data transformation** using AWS Lambda. The Lambda function can process the incoming data, remove the customer identifiers, and return the modified data.
- **Lambda Function**: The developer can write a Lambda function to identify and remove pattern-based customer identifiers from the data.
- **S3 Destination**: After transformation, the modified data can be stored in an S3 bucket, which is a supported destination for Kinesis Data Firehose.

#### **Why other options are incorrect**

- **Option B**: Using an EC2 instance as the destination adds unnecessary complexity and cost. Kinesis Data Firehose natively supports data transformation with Lambda, making this approach inefficient.
- **Option C**: Amazon OpenSearch Service is not designed for real-time data transformation. It is used for search and analytics, not for modifying data before storage.
- **Option D**: AWS Step Functions is not a supported destination for Kinesis Data Firehose. It is used for orchestrating workflows, not for real-time data transformation.

#### **Key Takeaways**

- **Kinesis Data Firehose Transformation**: Use a Lambda function to transform data in real time as it flows through the delivery stream.
- **Lambda Function**: Write a Lambda function to remove customer identifiers from the data.
- **S3 Destination**: Store the transformed data in an S3 bucket for further processing or analysis.

By implementing **Kinesis Data Firehose data transformation with a Lambda function**, the developer can efficiently remove customer identifiers and store the modified data in S3.

</details>

## Question #41

A developer is using an AWS Lambda function to generate avatars for profile pictures that are uploaded to an Amazon S3 bucket. The Lambda function is automatically invoked for profile pictures that are saved under the `/original/` S3 prefix. The developer notices that some pictures cause the Lambda function to time out. The developer wants to implement a fallback mechanism by using another Lambda function that resizes the profile picture.  
Which solution will meet these requirements with the LEAST development effort?

- A. Set the image resize Lambda function as a destination of the avatar generator Lambda function for the events that fail processing.
- B. Create an Amazon Simple Queue Service (Amazon SQS) queue. Set the SQS queue as a destination with an on failure condition for the avatar generator Lambda function. Configure the image resize Lambda function to poll from the SQS queue.
- C. Create an AWS Step Functions state machine that invokes the avatar generator Lambda function and uses the image resize Lambda function as a fallback. Create an Amazon EventBridge rule that matches events from the S3 bucket to invoke the state machine.
- D. Create an Amazon Simple Notification Service (Amazon SNS) topic. Set the SNS topic as a destination with an on failure condition for the avatar generator Lambda function. Subscribe the image resize Lambda function to the SNS topic.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Set the image resize Lambda function as a destination of the avatar generator Lambda function for the events that fail processing.**

### Explanation

#### **Purpose of Fallback Mechanism**

The developer needs to handle cases where the avatar generator Lambda function times out by invoking a fallback Lambda function to resize the profile picture. This should be done with minimal development effort.

#### **Why this option is correct**

- **Lambda Destinations**: Lambda supports **destinations** for asynchronous invocations. You can configure a destination for events that fail processing, such as timeouts or errors.
- **Least Development Effort**: By setting the image resize Lambda function as a destination for failed events, the developer can implement the fallback mechanism without additional infrastructure or complex configurations.
- **Automatic Invocation**: When the avatar generator Lambda function fails, the image resize Lambda function is automatically invoked, ensuring that the profile picture is processed.

#### **Why other options are incorrect**

- **Option B**: Using an SQS queue requires additional setup, such as creating the queue and configuring the image resize Lambda function to poll from it. This adds complexity and development effort.
- **Option C**: Using AWS Step Functions and EventBridge introduces unnecessary complexity for this use case. It requires creating a state machine and configuring EventBridge rules.
- **Option D**: Using an SNS topic requires additional setup, such as creating the topic and subscribing the image resize Lambda function to it. This adds complexity and development effort.

#### **Key Takeaways**

- **Lambda Destinations**: Use Lambda destinations to automatically invoke a fallback function when the primary function fails.
- **Minimal Effort**: This approach requires the least development effort and no additional infrastructure.
- **Automatic Fallback**: The fallback function is automatically invoked, ensuring that the profile picture is processed.

By setting the **image resize Lambda function as a destination for failed events**, the developer can implement the fallback mechanism with minimal effort.

</details>

## Question #42

A company is running an application on Amazon Elastic Container Service (Amazon ECS). When the company deploys a new version of the application, the company initially needs to expose 10% of live traffic to the new version. After a period of time, the company needs to immediately route all the remaining live traffic to the new version.  
Which ECS deployment should the company use to meet these requirements?

- A. Rolling update
- B. Blue/green with canary
- C. Blue/green with all at once
- D. Blue/green with linear

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Blue/green with canary**

### Explanation

#### **Purpose of the Deployment Strategy**

The company requires a deployment approach that allows a gradual exposure of 10% of live traffic to the new application version for testing, followed by an immediate shift of all remaining traffic to the new version once validated.

#### **Why this option is correct**

- **Blue/Green with Canary**: In a blue/green deployment, two environments (blue and green) run simultaneously. The canary variant enables routing a small percentage of traffic (e.g., 10%) to the new version (green) while the old version (blue) handles the rest. After a validation period, all traffic can be instantly switched to the new version.
- **ECS Integration**: Amazon ECS supports blue/green deployments with AWS CodeDeploy, which offers a canary option to precisely control traffic distribution and perform an immediate full switch when ready.
- **Meets Requirements**: This approach satisfies both the initial 10% traffic exposure and the subsequent immediate full traffic shift with minimal manual intervention.

#### **Why other options are incorrect**

- **Option A**: A rolling update replaces tasks incrementally based on health checks and percentages, but it doesn’t allow precise traffic control (e.g., exactly 10%) or an immediate full switch, making it unsuitable.
- **Option C**: Blue/green with all-at-once shifts all traffic to the new version instantly after deployment, skipping the 10% gradual exposure phase, which fails the first requirement.
- **Option D**: Blue/green with linear gradually increases traffic to the new version (e.g., 10% every few minutes), but it doesn’t support an immediate full switch after the initial phase, conflicting with the second requirement.

#### **Key Takeaways**

- **Controlled Rollout**: The canary approach ensures safe testing with 10% traffic exposure before full deployment.
- **Instant Switch**: Blue/green enables an immediate traffic shift to the new version after validation.
- **ECS Compatibility**: Leverages AWS CodeDeploy’s canary feature for seamless execution on ECS.

By using **blue/green with canary**, the company can efficiently manage the initial traffic exposure and fully transition to the new version with confidence.

</details>

## Question #43

A microservices application is deployed across multiple containers in Amazon Elastic Container Service (Amazon ECS). To improve performance, a developer wants to capture trace information between the microservices and visualize the microservices architecture.  
Which solution will meet these requirements?

- A. Build the container from the amazon/aws-xray-daemon base image. Use the AWS X-Ray SDK to instrument the application.
- B. Install the Amazon CloudWatch agent on the container image. Use the CloudWatch SDK to publish custom metrics from each of the microservices.
- C. Install the AWS X-Ray daemon on each of the ECS instances.
- D. Configure AWS CloudTrail data events to capture the traffic between the microservices.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Build the container from the amazon/aws-xray-daemon base image. Use the AWS X-Ray SDK to instrument the application.**

### Explanation

#### **Purpose of Tracing and Visualization**

The developer aims to enhance performance by capturing trace information—details of how requests flow between microservices—and visualizing the architecture to identify bottlenecks and dependencies. This requires a solution that tracks request paths across containers and provides a graphical representation of the microservices.

#### **Why this option is correct**

- **AWS X-Ray Integration**: AWS X-Ray is purpose-built for tracing requests across distributed systems like microservices and generating service maps for visualization. Building the container from the `amazon/aws-xray-daemon` base image includes the X-Ray daemon, which collects trace data, while the AWS X-Ray SDK instruments the application to generate and send this data.
- **Comprehensive Solution**: This approach ensures both components (daemon and SDK) work together within the container, capturing traces and enabling visualization via the X-Ray console’s service map, directly meeting the requirements.
- **ECS Compatibility**: Works seamlessly with ECS, whether on EC2 or Fargate, as the tracing logic is embedded in the container, simplifying deployment for developers.

#### **Why other options are incorrect**

- **Option B**: Installing the Amazon CloudWatch agent and using the CloudWatch SDK focuses on collecting metrics and logs, not tracing request paths across microservices. While useful for monitoring, CloudWatch lacks the detailed tracing and service map visualization provided by X-Ray, failing to meet the requirements.
- **Option C**: Installing the AWS X-Ray daemon on each ECS instance assumes an EC2-based setup and doesn’t work with Fargate, where instances are abstracted. Additionally, it requires separate application instrumentation (not mentioned), making it incomplete and less universal compared to Option A.
- **Option D**: AWS CloudTrail captures API calls to AWS services for auditing, not application-level traffic between microservices. It offers no tracing or visualization capabilities relevant to this use case, rendering it unsuitable.

#### **Key Takeaways**

- **Tracing with X-Ray**: The combination of the X-Ray daemon and SDK provides end-to-end request tracing across microservices.
- **Visualization**: AWS X-Ray’s service map visually represents the architecture, aiding performance optimization.
- **Simplified Setup**: Building the container with the daemon and SDK offers a developer-friendly, self-contained solution for ECS.

By building the container from the **amazon/aws-xray-daemon base image and using the AWS X-Ray SDK**, the developer can effectively capture trace information and visualize the microservices architecture, improving performance with a straightforward implementation.

</details>

## Question #44

A developer notices timeouts from the AWS CLI when the developer runs list commands.  
What should the developer do to avoid these timeouts?

- A. Use the `--page-size` parameter to request a smaller number of items.
- B. Use shorthand syntax to separate the list by a single space.
- C. Use the `yaml-stream` output for faster viewing of large datasets.
- D. Use quotation marks around strings to enclose data structure.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Use the `--page-size` parameter to request a smaller number of items.**

### Explanation

#### **Purpose of Avoiding Timeouts**

The developer is experiencing timeouts when running AWS CLI list commands, likely due to large datasets being retrieved from AWS services (e.g., `aws s3 ls`, `aws ec2 describe-instances`). The goal is to reduce the response time or server load to prevent these timeouts.

#### **Why this option is correct**

- **Pagination Control**: The `--page-size` parameter allows the developer to specify a smaller number of items returned per API call. While the total data retrieved remains the same, smaller pages reduce the server-side processing time per request, making it less likely to hit timeout limits.
- **Direct Impact on Timeouts**: By requesting fewer items at once (e.g., `--page-size 50` instead of the default `1000`), the AWS CLI can process responses more quickly, avoiding timeouts, especially with services that have large result sets.
- **AWS CLI Feature**: This is a standard parameter supported across many AWS CLI commands that use pagination, making it a practical and effective solution.

#### **Why other options are incorrect**

- **Option B**: Using shorthand syntax (e.g., separating values with spaces) applies to command input, not output retrieval. It doesn’t affect how data is fetched from AWS services or reduce timeouts, making it irrelevant to this issue.
- **Option C**: The `yaml-stream` output format streams results as YAML, which might help with viewing large datasets incrementally on the client side, but it doesn’t change the server-side retrieval process or reduce the likelihood of timeouts from the AWS API.
- **Option D**: Adding quotation marks around strings is a syntax requirement for certain CLI inputs, not a solution for timeouts. It has no impact on the performance of list commands or data retrieval.

#### **Key Takeaways**

- **Efficient Pagination**: The `--page-size` parameter optimizes data retrieval by breaking it into manageable chunks, reducing server load and response time.
- **Timeout Prevention**: Smaller page sizes directly address the root cause of timeouts in large list operations.
- **Simple Implementation**: This requires only a minor adjustment to existing commands (e.g., `aws s3 ls --page-size 50`), making it an easy fix.

By using the **`--page-size` parameter to request a smaller number of items**, the developer can avoid timeouts and improve the reliability of AWS CLI list commands.

</details>

## Question #45

A company has moved a legacy on-premises application to AWS by performing a lift and shift. The application exposes a REST API that can be used to retrieve billing information. The application is running on a single `Amazon EC2` instance. The application code cannot support concurrent invocations. Many clients access the API, and the company adds new clients all the time.  
A developer is concerned that the application might become overwhelmed by too many requests. The developer needs to limit the number of requests to the API for all current and future clients. The developer must not change the API, the application, or the client code.  
What should the developer do to meet these requirements?

- A. Place the API behind an `Amazon API Gateway` API. Set the server-side throttling limits.
- B. Place the API behind a `Network Load Balancer`. Set the target group throttling limits.
- C. Place the API behind an `Application Load Balancer`. Set the target group throttling limits.
- D. Place the API behind an `Amazon API Gateway` API. Set the per-client throttling limits.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**A. Place the API behind an `Amazon API Gateway` API. Set the server-side throttling limits.**

### Explanation

#### **Purpose of Request Limiting**

The developer needs to protect a legacy application on a single `Amazon EC2` instance, which cannot handle concurrent invocations, from being overwhelmed by API requests. The solution must limit requests for all clients (current and future) without modifying the application, API, or client code, requiring an external mechanism to enforce throttling.

#### **Why this option is correct**

- **`Amazon API Gateway` Throttling**: `Amazon API Gateway` provides built-in server-side throttling limits, allowing the developer to set a maximum request rate (e.g., requests per second) and burst capacity for the entire API. This applies uniformly to all clients accessing the API through the gateway.
- **No Code Changes**: By placing the API behind `Amazon API Gateway`, the existing REST API on the `EC2` instance remains unchanged. The gateway acts as a proxy, forwarding requests to the `EC2` instance while enforcing throttling, meeting the constraint of not modifying the application or client code.
- **Scalability for All Clients**: Server-side throttling ensures a consistent limit across all current and future clients without needing per-client configuration, aligning with the requirement to handle an growing client base.

#### **Why other options are incorrect**

- **Option B**: A `Network Load Balancer` operates at the transport layer (Layer 4) and does not natively support request throttling or rate limiting for APIs. There are no "target group throttling limits" available, making this option invalid for controlling API request rates.
- **Option C**: An `Application Load Balancer` (Layer 7) can distribute traffic but does not offer built-in throttling or rate-limiting features at the target group level. It’s designed for load distribution, not request limiting, so it doesn’t meet the requirements.
- **Option D**: While `Amazon API Gateway` supports per-client throttling (via usage plans and API keys), this requires client-specific configuration and potentially modifying clients to include API keys. This conflicts with the requirement to avoid client code changes and handle future clients seamlessly, making server-side throttling (Option A) more appropriate.

#### **Key Takeaways**

- **Effective Throttling**: `Amazon API Gateway`’s server-side throttling protects the application by limiting total request rates, preventing overload on the single-threaded legacy app.
- **No Modification Needed**: The solution works externally to the application and clients, preserving the lift-and-shift approach.
- **Universal Application**: Server-side limits apply to all clients without individual setup, ideal for a dynamic client base.

By placing the API behind an **`Amazon API Gateway` API and setting server-side throttling limits**, the developer can safeguard the application from excessive requests while meeting all specified constraints.

</details>

## Question #46

A developer has created a web API that uses `Amazon Elastic Container Service` (`Amazon ECS`) and an `Application Load Balancer` (`ALB`). An `Amazon CloudFront` distribution uses the API as an origin for web clients. The application has received millions of requests with a JSON Web Token (JWT) that is not valid in the authorization header. The developer has scaled out the application to handle the unauthenticated requests.  
What should the developer do to reduce the number of unauthenticated requests to the API?

- A. Add a request routing rule to the `ALB` to return a `401` status code if the authorization header is missing.
- B. Add a container to the `ECS` task definition to validate JWTs. Set the new container as a dependency of the application container.
- C. Create a `CloudFront` function for the distribution. Use the `crypto` module in the function to validate the JWT.
- D. Add a custom authorizer for `AWS Lambda` to the `CloudFront` distribution to validate the JWT.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Create a `CloudFront` function for the distribution. Use the `crypto` module in the function to validate the JWT.**

### Explanation

#### **Purpose of Reducing Unauthenticated Requests**

The developer needs to reduce the load of unauthenticated requests—those with invalid JWTs in the authorization header—reaching the `Amazon ECS`-based API behind the `ALB`. Since the application is scaling to handle these requests, filtering them out earlier in the request pipeline will improve efficiency and reduce costs without modifying the core application.

#### **Why this option is correct**

- **`CloudFront` Functions**: `Amazon CloudFront` supports `CloudFront Functions`, lightweight JavaScript functions that run at the edge. The developer can inspect the `Authorization` header, use the `crypto` module (available in `CloudFront Functions`) to validate the JWT signature, and reject invalid requests with a `401 Unauthorized` response before they reach the origin (`ALB` and `ECS`).
- **Early Filtering**: By validating JWTs at the `CloudFront` edge, unauthenticated requests are blocked globally across CloudFront’s edge locations, significantly reducing the number of requests hitting the `ALB` and `ECS`, thus minimizing scaling needs and costs.
- **No Application Changes**: This solution requires no modifications to the existing `ECS` application or `ALB` configuration, aligning with a low-impact approach to address the issue.

#### **Why other options are incorrect**

- **Option A**: Adding a rule to the `ALB` to return a `401` status code if the `Authorization` header is missing doesn’t fully address the problem. The issue is invalid JWTs, not missing headers. The `ALB` cannot validate JWT signatures natively, so this only catches a subset of unauthenticated requests, allowing invalid JWTs to still reach the application.
- **Option B**: Adding a container to the `ECS` task definition to validate JWTs and setting it as a dependency shifts the validation burden to the application layer. While effective, it requires redeploying the `ECS` service, managing container dependencies, and still allows requests to reach the `ALB` and `ECS`, increasing operational complexity and not reducing traffic as efficiently as edge filtering.
- **Option D**: Adding an `AWS Lambda` custom authorizer to the `CloudFront` distribution is feasible but overkill. `Lambda@Edge` can validate JWTs, but it’s heavier and more expensive than `CloudFront Functions`, which are designed for simple request manipulation tasks like this. Additionally, `CloudFront` doesn’t natively integrate authorizers the way `API Gateway` does, making this less straightforward.

#### **Key Takeaways**

- **Edge Validation**: Using a `CloudFront` function with the `crypto` module validates JWTs at the edge, stopping unauthenticated requests before they reach the origin.
- **Efficiency**: Reduces the load on `ALB` and `ECS`, avoiding unnecessary scaling and lowering costs.
- **Simplicity**: Requires minimal setup—a small JavaScript function—without altering the existing infrastructure or application.

By creating a **`CloudFront` function for the distribution and using the `crypto` module to validate the JWT**, the developer can effectively reduce unauthenticated requests to the API at the earliest point in the request flow.

</details>

## Question #47

A developer has created an `AWS Lambda` function that uses 15 MB of memory. When the developer runs the code natively on a laptop that has 4 cores, the function runs within 100 ms. When the developer deploys the code as an `AWS Lambda` function with 128 MB of memory, the first run takes 3 seconds. Subsequent runs take more than 500 ms to finish.  
The developer needs to improve the performance of the `AWS Lambda` function so that the function runs consistently in less than 100 ms, excluding the initial startup time.  
Which solution will meet this requirement?

- A. Increase the reserved concurrency of the `AWS Lambda` function.
- B. Increase the provisioned concurrency of the `AWS Lambda` function.
- C. Increase the memory of the `AWS Lambda` function.
- D. Repackage the `AWS Lambda` function as a container. Redeploy the function.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Increase the memory of the `AWS Lambda` function.**

### Explanation

#### **Purpose of Improving Performance**

The developer aims to optimize the `AWS Lambda` function’s execution time to consistently run in less than 100 ms, excluding the initial cold start (3 seconds). The current setup with 128 MB of memory results in subsequent warm runs taking over 500 ms, compared to 100 ms on a laptop, indicating a performance bottleneck in the Lambda environment.

#### **Why this option is correct**

- **Memory and CPU Correlation**: In `AWS Lambda`, memory allocation directly affects CPU power. At 128 MB, the function gets a fraction of a vCPU, which limits its compute capacity. Increasing memory (e.g., to 256 MB or higher) proportionally increases CPU allocation, enabling faster execution.
- **Execution Time Impact**: Since the code runs in 100 ms natively with more CPU resources (4 cores), the Lambda function’s 500 ms warm runtime suggests insufficient compute power. Boosting memory addresses this by providing more CPU, likely reducing runtime below 100 ms for subsequent invocations.
- **Excludes Cold Start**: The requirement focuses on consistent performance after the initial startup (cold start), which is addressed by optimizing warm execution time through memory adjustment, not concurrency settings.

#### **Why other options are incorrect**

- **Option A**: Increasing reserved concurrency sets aside a specific number of execution environments for the function, ensuring availability but not improving individual runtime performance. It doesn’t address the 500 ms warm execution time, failing to meet the sub-100 ms goal.
- **Option B**: Provisioned concurrency pre-warms execution environments to reduce cold start latency (e.g., the 3-second first run), but it doesn’t enhance the performance of subsequent warm runs, which still take over 500 ms. The requirement excludes cold start time, making this irrelevant.
- **Option D**: Repackaging as a container allows custom runtimes but doesn’t inherently improve execution speed unless optimized for performance (e.g., with more memory or CPU tweaks). It adds complexity without guaranteeing sub-100 ms runtime, unlike simply increasing memory.

#### **Key Takeaways**

- **Memory Drives Performance**: Higher memory in `AWS Lambda` allocates more CPU, directly reducing execution time for compute-bound tasks.
- **Warm Run Focus**: Adjusting memory targets the 500 ms warm runtime, aligning it with the 100 ms laptop benchmark.
- **Simple Solution**: Increasing memory is a configuration change (e.g., from 128 MB to 512 MB), requiring no code or deployment overhaul.

By increasing the memory of the **`AWS Lambda` function**, the developer can enhance its compute power and achieve consistent execution times below 100 ms for subsequent runs, meeting the performance requirement efficiently.

</details>

## Question #48

`Amazon Kinesis` is used to load data into a stock market monitoring application. The `Kinesis` stream cannot keep up with the incoming data during simulated peak data rates testing.  
What step will enable `Kinesis` to handle peak-hour traffic?

- A. Install the `Kinesis Producer Library` (`KPL`) for ingesting data into the stream.
- B. Reduce the data retention period to allow for more data ingestion using `DecreaseStreamRetentionPeriod`.
- C. Increase the shard count of the stream using `UpdateShardCount`.
- D. Ingest multiple records into the stream in a single call using `PutRecords`.

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. Increase the shard count of the stream using `UpdateShardCount`.**

### Explanation

#### **Purpose of Handling Peak Traffic**

The `Amazon Kinesis` stream is failing to process incoming data during peak simulated rates, indicating that its throughput capacity is insufficient. The goal is to enhance the stream’s ability to ingest and process data to accommodate high traffic volumes, such as during peak stock market activity.

#### **Why this option is correct**

- **Shard-Based Throughput**: In `Amazon Kinesis Data Streams`, throughput is determined by the number of shards. Each shard supports up to 1 MB/s or 1,000 records/s for ingestion and 2 MB/s for consumption. If the stream can’t keep up, it’s likely hitting these limits, and adding shards increases capacity proportionally.
- **`UpdateShardCount`**: The `UpdateShardCount` API allows the developer to dynamically increase the number of shards in the stream, directly boosting ingestion and processing capacity to handle peak-hour traffic without changing the application or producer logic.
- **Direct Solution**: This addresses the root cause—insufficient stream capacity—ensuring `Kinesis` can scale to meet demand during testing and real-world peaks.

#### **Why other options are incorrect**

- **Option A**: Installing the `Kinesis Producer Library` (`KPL`) improves producer efficiency by batching and compressing data, but it doesn’t increase the stream’s inherent capacity. If the stream’s shard limits are already saturated, `KPL` won’t resolve the bottleneck.
- **Option B**: Reducing the data retention period via `DecreaseStreamRetentionPeriod` (e.g., from 24 hours to 1 hour) frees up storage but has no impact on ingestion throughput or shard capacity. It’s irrelevant to handling peak traffic rates.
- **Option D**: Using `PutRecords` to ingest multiple records in a single call optimizes producer-side efficiency by reducing API calls, but it doesn’t expand the stream’s capacity. If the shard ingestion limit (1 MB/s or 1,000 records/s) is exceeded, this won’t prevent the stream from falling behind.

#### **Key Takeaways**

- **Scalability with Shards**: Increasing the shard count via `UpdateShardCount` directly scales `Kinesis` throughput to match peak data rates.
- **Capacity Focus**: The solution targets the stream’s ability to ingest data, not just producer efficiency or retention settings.
- **Simple Adjustment**: This requires a single API call or configuration change, minimizing operational overhead.

By increasing the shard count of the stream using **`UpdateShardCount`**, the developer can enable `Amazon Kinesis` to handle peak-hour traffic effectively, ensuring the stock market monitoring application processes data without delays.

</details>

## Question #49

A developer must install a serverless RESTful API on `AWS` regularly and consistently.  
Which strategies will be effective? (Select two.)

- A. Define a `Swagger` file. Use `AWS Elastic Beanstalk` to deploy the `Swagger` file.
- B. Define a `Swagger` file. Use `AWS CodeDeploy` to deploy the `Swagger` file.
- C. Deploy a `SAM` template with an inline `Swagger` definition.
- D. Define a `Swagger` file. Deploy a `SAM` template that references the `Swagger` file.
- E. Define an inline `Swagger` definition in a `Lambda` function. Invoke the `Lambda` function.

<details>
<summary>Answer</summary>
<br>

The correct answers are:

**C. Deploy a `SAM` template with an inline `Swagger` definition.**  
**D. Define a `Swagger` file. Deploy a `SAM` template that references the `Swagger` file.**

### Explanation

#### **Purpose of Serverless API Deployment**

The developer needs to install a serverless RESTful API on `AWS` in a regular and consistent manner, implying a repeatable, automated process suitable for serverless architecture (e.g., `AWS Lambda` and `Amazon API Gateway`). Effective strategies must leverage tools designed for serverless deployments and support API definitions like `Swagger` (OpenAPI).

#### **Why these options are correct**

- **Option C: `SAM` with Inline `Swagger`**

  - **`AWS SAM` Advantage**: The `AWS Serverless Application Model` (`SAM`) is purpose-built for deploying serverless applications, including `AWS Lambda` functions and `Amazon API Gateway` APIs. It supports defining RESTful APIs directly in the template.
  - **Inline `Swagger`**: Including a `Swagger` definition within the `SAM` template (e.g., under the `DefinitionBody` property of an `AWS::Serverless::Api` resource) allows the developer to specify the API’s endpoints, methods, and `Lambda` integrations in a single, consistent file. This ensures repeatable deployments via `SAM` CLI or CI/CD pipelines.
  - **Consistency**: The inline approach keeps everything self-contained, reducing external dependencies and simplifying version control.

- **Option D: `SAM` Referencing `Swagger` File**
  - **External `Swagger` File**: Defining the API in a separate `Swagger` file and referencing it in the `SAM` template (via the `DefinitionUri` property) separates API design from infrastructure code, promoting modularity.
  - **`SAM` Deployment**: `SAM` still handles the deployment of `Lambda` and `API Gateway`, using the referenced `Swagger` file to configure the API. This method is consistent and repeatable, leveraging `SAM`’s serverless capabilities while allowing the `Swagger` file to be maintained independently.
  - **Regular Use**: The developer can update the `Swagger` file and redeploy with `SAM`, ensuring regular, predictable updates.

#### **Why other options are incorrect**

- **Option A**: `AWS Elastic Beanstalk` is designed for traditional web applications (e.g., containers or VMs), not serverless architectures. It doesn’t natively support `Swagger` for `API Gateway` or serverless deployments, making it unsuitable for this requirement.
- **Option B**: `AWS CodeDeploy` is for deploying code to `EC2`, `ECS`, or `Lambda`, but it doesn’t directly manage `API Gateway` or interpret `Swagger` files for serverless RESTful APIs. It’s not a complete or consistent solution for this use case.
- **Option E**: Defining a `Swagger` definition inside a `Lambda` function and invoking it doesn’t deploy a RESTful API. `Swagger` is a specification for API structure, not executable code, and this approach misunderstands its purpose, failing to create an API endpoint.

#### **Key Takeaways**

- **Serverless Focus**: Both `SAM`-based options (C and D) align with `AWS` serverless best practices, using `Lambda` and `API Gateway` effectively.
- **Consistency**: `SAM` templates ensure repeatable deployments, whether `Swagger` is inline or referenced, supporting regular updates.
- **Flexibility**: Option C is self-contained, while Option D separates API design, both meeting the goal with minimal overhead.

By deploying a **`SAM` template with an inline `Swagger` definition** (C) or defining a `Swagger` file and deploying a **`SAM` template that references it** (D), the developer can install a serverless RESTful API on `AWS` regularly and consistently.

</details>

## Question #50

Your company has a bucket that has versioning and encryption enabled. The bucket receives thousands of `PUT` operations per day. After 6 months, a significant number of `HTTP 503` error codes are being received.  
Which of the following can be used to diagnose the error?

- A. `AWS Config`
- B. `AWS CloudTrail`
- C. `Amazon S3 Inventory`
- D. `AWS Trusted Advisor`

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**C. `Amazon S3 Inventory`**

### Explanation

#### **Purpose of Diagnosing HTTP 503 Errors**

The company’s `Amazon S3` bucket, with versioning and encryption enabled, is experiencing `HTTP 503 Service Unavailable` errors after 6 months of thousands of daily `PUT` operations. A `503` error often indicates throttling due to excessive request rates per prefix, exacerbated by versioning, which generates additional object versions. The goal is to diagnose the issue, and the question does not require real-time analysis, allowing for a broader interpretation of diagnostic tools.

#### **Why this option is correct**

- **`Amazon S3 Inventory` Capabilities**: `Amazon S3 Inventory` generates periodic reports (e.g., daily or weekly) listing all objects and their versions in the bucket, including metadata like size, version ID, and encryption status. Over 6 months, versioning could have led to a massive accumulation of object versions, increasing the complexity of `PUT` operations and contributing to throttling.
- **Diagnosing Versioning Impact**: By analyzing the inventory report, the developer can assess the number of versions per object or prefix, identifying if excessive versioning is causing `S3` to hit performance limits (e.g., 3,500 `PUT` requests per second per prefix with versioning). This insight helps diagnose whether the `503` errors stem from prefix overload or version sprawl.
- **No Real-Time Requirement**: Since the question doesn’t mandate real-time analysis, `S3 Inventory`’s scheduled reporting is sufficient to investigate historical trends leading to the errors, aligning with the long-term context (6 months).

#### **Why other options are incorrect**

- **Option A**: `AWS Config` monitors configuration changes to `AWS` resources (e.g., bucket settings) but doesn’t provide data on object versions, request rates, or `503` errors, making it unsuitable for diagnosing this issue.
- **Option B**: `AWS CloudTrail` logs API calls, including `PUT` operations and `503` responses, offering real-time diagnostics. However, without a real-time requirement, its focus on request-level detail is less directly tied to versioning’s cumulative impact compared to `S3 Inventory` in this context.
- **Option D**: `AWS Trusted Advisor` offers high-level recommendations (e.g., performance checks) but lacks detailed diagnostics for `S3` versioning or throttling issues, rendering it ineffective here.

#### **Key Takeaways**

- **Versioning Insight**: `Amazon S3 Inventory` reveals the extent of version accumulation, a potential cause of `503` errors under high `PUT` loads.
- **Diagnostic Fit**: It provides a historical view of bucket contents, suitable for diagnosing issues built up over 6 months without needing real-time data.
- **Practical Approach**: The developer can use inventory reports to identify problematic prefixes or version counts, informing mitigation like lifecycle policies.

By using **`Amazon S3 Inventory`**, the developer can diagnose the `HTTP 503` errors by analyzing the impact of versioning and high `PUT` operation volumes on the bucket over time, addressing the issue effectively within the given context.

</details>

## Question #51

Your company has an application that is interacting with an `Amazon DynamoDB` table. After reviewing the logs for the application, it has been noticed that there are quite a few `ProvisionedThroughputExceededException` errors occurring in the logs.  
Which of the following can be implemented to overcome these errors?

- A. Implement global tables
- B. Use exponential backoff in the program
- C. Ensure the correct permissions are set for the instance profile for the instance hosting the application
- D. Ensure to use indexes instead

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Use exponential backoff in the program**

### Explanation

#### **Purpose of Overcoming Throughput Errors**

The application interacting with an `Amazon DynamoDB` table is encountering `ProvisionedThroughputExceededException` errors, indicating that the request rate exceeds the table’s provisioned read or write capacity units (RCUs/WCUs). The solution must mitigate these errors by managing request rates effectively without necessarily increasing costs or changing the table’s structure immediately.

#### **Why this option is correct**

- **Exponential Backoff Mechanism**: Implementing exponential backoff in the application involves retrying failed `DynamoDB` requests with increasing delays (e.g., 50 ms, 100 ms, 200 ms) after a `ProvisionedThroughputExceededException`. This reduces the immediate retry rate, allowing the table’s provisioned capacity to handle the load over time.
- **Error Mitigation**: `DynamoDB` throttles requests when capacity is exceeded, and backoff helps smooth out request spikes, preventing repeated failures and giving the table breathing room to process requests within its limits.
- **AWS SDK Support**: The `AWS SDK` (e.g., for Python, Java) includes built-in retry logic with exponential backoff for `DynamoDB` operations, making this a straightforward code-level fix without altering infrastructure.

#### **Why other options are incorrect**

- **Option A**: Implementing global tables replicates the `DynamoDB` table across multiple `AWS` regions for low-latency access and disaster recovery. However, it doesn’t increase the provisioned throughput of the table in the primary region or address local capacity limits, so it won’t resolve the throughput exceptions.
- **Option C**: Ensuring correct permissions via the instance profile fixes authentication/authorization issues (e.g., `AccessDeniedException`), but `ProvisionedThroughputExceededException` is a capacity issue, not a permissions problem, making this irrelevant.
- **Option D**: Using indexes (e.g., Global Secondary Indexes or Local Secondary Indexes) optimizes query patterns but doesn’t inherently increase the table’s base throughput capacity. Indexes have their own provisioned capacity, and without adjusting it, throttling can still occur.

#### **Key Takeaways**

- **Rate Management**: Exponential backoff reduces the application’s request rate during throttling, aligning it with the table’s provisioned capacity.
- **Code-Level Fix**: This solution requires only a programming change, avoiding infrastructure modifications or additional costs (unlike increasing RCUs/WCUs).
- **Best Practice**: Backoff is an `AWS`-recommended strategy for handling `DynamoDB` throttling, enhancing application resilience.

By using **exponential backoff in the program**, the developer can overcome `ProvisionedThroughputExceededException` errors, ensuring the application interacts with the `Amazon DynamoDB` table reliably within its capacity limits.

</details>

## Question #52

You are part of a development team that is in charge of creating `AWS CloudFormation` templates. These templates need to be created across multiple accounts with the least amount of effort.  
Which of the following would assist in accomplishing this?

- A. Creating `CloudFormation` ChangeSets
- B. Creating `CloudFormation` StackSets
- c. Make use of Nested stacks
- D. Use `CloudFormation` artifacts

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**B. Creating `CloudFormation` StackSets**

### Explanation

#### **Purpose of Multi-Account Template Deployment**

The team needs to deploy `AWS CloudFormation` templates across multiple `AWS` accounts efficiently, minimizing effort. This requires a solution that automates and centralizes the deployment process across accounts, rather than manually applying templates in each one.

#### **Why this option is correct**

- **`CloudFormation` StackSets**: `AWS CloudFormation StackSets` allows the team to deploy a single `CloudFormation` template (or stack) across multiple accounts and regions from a central management account. It automates the creation and management of stacks, ensuring consistency and reducing manual effort.
- **Least Effort**: With `StackSets`, the team defines the template once and specifies target accounts and regions. `AWS` handles the deployment, updates, and drift detection across all specified accounts, eliminating the need to replicate work manually.
- **Scalability**: As the number of accounts grows, `StackSets` scales effortlessly, making it ideal for multi-account environments like those in an `AWS Organizations` setup.

#### **Why other options are incorrect**

- **Option A**: `CloudFormation` ChangeSets preview changes to an existing stack within a single account before applying them. They don’t assist in deploying templates across multiple accounts, failing the multi-account requirement.
- **Option c**: Nested stacks organize complex templates by breaking them into reusable sub-stacks within a single account or region. While useful for modularity, they don’t address deployment across multiple accounts, requiring additional effort for each account.
- **Option D**: `CloudFormation` artifacts typically refer to files (e.g., templates, Lambda code) stored in `Amazon S3` for deployment. While artifacts support template creation, they don’t inherently facilitate multi-account deployment, leaving the effort of applying them to each account unchanged.

#### **Key Takeaways**

- **Centralized Deployment**: `CloudFormation StackSets` streamlines template deployment across multiple accounts from a single point of control.
- **Effort Reduction**: It automates the process, minimizing manual intervention compared to deploying stacks individually.
- **Multi-Account Design**: Built for scenarios like this, `StackSets` aligns with `AWS` best practices for cross-account infrastructure management.

By creating **`CloudFormation` StackSets**, the development team can deploy `AWS CloudFormation` templates across multiple accounts with the least amount of effort, meeting the requirement efficiently.

</details>

## Question #53

Your team is looking towards uploading different versions of an application using `AWS Elastic Beanstalk`.  
How can they achieve this in the easiest possible way?

- A. Create multiple applications in `Elastic Beanstalk`
- B. Create multiple environments in `Elastic Beanstalk`
- c. Upload the application's source code or source bundle in the `Elastic Beanstalk` console
- D. Use `AWS CodePipeline` to streamline the various application versions

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**c. Upload the application's source code or source bundle in the `Elastic Beanstalk` console**

### Explanation

#### **Purpose of Uploading Application Versions**

The team wants to upload different versions of an application to `AWS Elastic Beanstalk` in the simplest way possible. This implies a straightforward method to manage and deploy multiple versions without unnecessary complexity or additional infrastructure setup.

#### **Why this option is correct**

- **Direct Upload Process**: Uploading the application’s source code or source bundle (e.g., a `.zip` file) via the `Elastic Beanstalk` console is the easiest built-in method. In the console, the team can navigate to an existing application, select the “Versions” tab, and upload a new version, which `Elastic Beanstalk` stores and tracks automatically.
- **Minimal Effort**: This approach requires no additional configuration beyond preparing the source bundle. Once uploaded, the team can deploy the version to an existing environment with a single click, making it ideal for quick iteration and testing of different versions.
- **`Elastic Beanstalk` Native Support**: `Elastic Beanstalk` inherently supports versioning, storing each uploaded bundle in an `Amazon S3` bucket managed by the service, and allowing easy switching between versions without creating new applications or environments.

#### **Why other options are incorrect**

- **Option A**: Creating multiple applications in `Elastic Beanstalk` means setting up entirely separate application entities, each with its own environments and configurations. This is overkill for managing versions of the same application and adds unnecessary complexity.
- **Option B**: Creating multiple environments (e.g., dev, test, prod) within an application allows running different versions simultaneously, but it’s not the easiest way to _upload_ versions. It requires additional setup and management compared to simply uploading via the console.
- **Option D**: Using `AWS CodePipeline` integrates `Elastic Beanstalk` with a CI/CD pipeline, automating deployments of various versions. While powerful, it involves configuring a pipeline, source repository, and build stages, which is more complex and not the easiest method for basic version uploads.

#### **Key Takeaways**

- **Simplicity**: Uploading via the `Elastic Beanstalk` console is a manual, user-friendly process requiring no advanced setup or tools.
- **Version Management**: `Elastic Beanstalk` handles version tracking natively, making it easy to deploy or roll back to any uploaded version.
- **Immediate Use**: The team can achieve their goal directly through the console, aligning with the requirement for the easiest approach.

By uploading the application's source code or source bundle in the **`Elastic Beanstalk` console**, the team can manage different versions of their application in the simplest possible way, leveraging `AWS Elastic Beanstalk`’s built-in functionality.

</details>

## Question #54

You are working as a Senior Software Developer for a large pharmaceutical company. Your lead has asked you to work on a new module in which you need to query an `Amazon DynamoDB` table with multiple Partition Key values at once and store the result in CSV format on `Amazon S3`.  
Which operation will you use to achieve the same?

- A. `Scan`
- B. `Query`
- c. `GetItem`
- D. `BatchGetItem`

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. `BatchGetItem`**

### Explanation

#### **Purpose of Querying Multiple Partition Keys**

The task requires querying an `Amazon DynamoDB` table for multiple Partition Key values in a single operation, then storing the results in CSV format on `Amazon S3`. This implies an efficient, batch-oriented approach to retrieve specific items based on their primary keys, followed by processing and export.

#### **Why this option is correct**

- **`BatchGetItem` Functionality**: The `BatchGetItem` operation in `Amazon DynamoDB` allows you to retrieve multiple items (up to 100 items or 16 MB of data) by specifying their Partition Key values (and Sort Key values, if applicable) in a single request. This is ideal for fetching data across multiple Partition Keys at once, as required.
- **Efficiency**: Unlike operations that retrieve one item at a time or scan the entire table, `BatchGetItem` targets specific keys, reducing latency and throughput consumption, making it suitable for the module’s needs.
- **Post-Processing**: After retrieving the items, you can process the results in your application code (e.g., using an `AWS SDK`) to format them as CSV and upload the file to `Amazon S3`, fulfilling the storage requirement.

#### **Why other options are incorrect**

- **Option A**: `Scan` reads every item in the table and filters them based on conditions, which is inefficient and costly for retrieving specific Partition Key values. It’s not designed for targeted queries and would return unnecessary data, failing the requirement for multiple specific keys.
- **Option B**: `Query` retrieves items based on a single Partition Key value and optional Sort Key conditions. It cannot query multiple Partition Keys at once in a single operation, making it unsuitable for this use case.
- **Option c**: `GetItem` fetches a single item by its Partition Key (and Sort Key, if applicable) in one request. It doesn’t support multiple Partition Keys simultaneously, requiring separate calls for each key, which is less efficient than `BatchGetItem`.

#### **Key Takeaways**

- **Batch Retrieval**: `BatchGetItem` efficiently queries multiple Partition Key values in one operation, meeting the requirement directly.
- **Targeted Access**: It avoids the overhead of scanning or querying irrelevant data, optimizing performance for specific key lookups.
- **Integration with S3**: The retrieved data can be easily formatted as CSV and stored in `Amazon S3` using standard coding practices.

By using **`BatchGetItem`**, you can query the `Amazon DynamoDB` table with multiple Partition Key values at once and store the result in CSV format on `Amazon S3`, achieving the module’s objectives efficiently.

</details>

## Question #55

You are a developer who is supporting a containerized application. You are told to set up dynamic port mapping for `Amazon ECS` and load balancing.  
What statement is true in this case?

- A. `Classic Load Balancer` allows you to run multiple copies of a task on the same instance
- B. `Application Load Balancer` uses static port mapping on a container instance
- C. After creating an `Amazon ECS` service, you add the load balancer configuration
- D. If dynamic port mapping is set up correctly, then you see the registered targets and the assigned port for the task

<details>
<summary>Answer</summary>
<br>

The correct answer is:

**D. If dynamic port mapping is set up correctly, then you see the registered targets and the assigned port for the task**

### Explanation

#### **Purpose of Dynamic Port Mapping and Load Balancing**

The task involves configuring dynamic port mapping for an `Amazon ECS` service with load balancing, typically using an `Application Load Balancer` (`ALB`). Dynamic port mapping allows `ECS` to assign ephemeral ports to container tasks, enabling flexible scaling and load distribution without requiring fixed port assignments.

#### **Why this option is correct**

- **Dynamic Port Mapping Behavior**: When dynamic port mapping is enabled in an `ECS` task definition (e.g., by setting the container’s host port to `0`), `ECS` automatically assigns a random available port on the container instance. The `ALB` integrates with this by registering the task as a target, associating it with the dynamically assigned port.
- **Visibility in Targets**: If set up correctly, the `ALB` target group shows the registered targets (the `ECS` tasks) along with their dynamically assigned ports (e.g., 32768–65535 range). This can be verified in the `AWS Management Console` or via the `describe-target-health` API, confirming proper configuration.
- **Load Balancing Integration**: The `ALB` routes traffic to these dynamic ports, ensuring the application is accessible, which aligns with the setup requirement.

#### **Why other options are incorrect**

- **Option A**: The `Classic Load Balancer` (CLB) doesn’t inherently support running multiple copies of a task on the same instance with dynamic port mapping in `ECS`. While it can work with static ports, `ECS` prefers `ALB` for dynamic mapping, and CLB is a legacy option not optimized for this use case, making this statement misleading.
- **Option B**: The `Application Load Balancer` (`ALB`) supports dynamic port mapping, not static port mapping, in `ECS`. Static port mapping requires predefined host ports, whereas `ALB` with `ECS` uses dynamic ports assigned by the service, so this statement is false.
- **Option C**: In `Amazon ECS`, the load balancer configuration (e.g., `ALB` target group) must be specified when creating the service, not added afterward. The service definition links the task to the load balancer during creation via the `loadBalancers` parameter, making this statement incorrect.

#### **Key Takeaways**

- **Dynamic Mapping Confirmation**: Correct setup of dynamic port mapping in `ECS` with an `ALB` results in visible registered targets and their assigned ports in the target group.
- **Load Balancer Fit**: `ALB` is the standard choice for dynamic port mapping in `ECS`, supporting flexible task scaling.
- **Setup Timing**: Load balancer integration is part of the `ECS` service creation, not a post-creation step.

If dynamic port mapping is set up correctly, then you see the registered targets and the assigned port for the task, making **D** the true statement in this context for `Amazon ECS` and load balancing.

</details>
