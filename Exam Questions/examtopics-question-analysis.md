## Question #01 - #01

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

## Question #02 - #02

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

## Question #03 - #03

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

## Question #04 - #04

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

## Question #05 - #05

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

## Question #06 - #06

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

## Question #07 - #07

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

## Question #08 - #08

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

## Question #09 - #09

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

## Question #10 - #10

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

## Question #11 - #11

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

## Question #12 - #12

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

## Question #13 - #13

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

## Question #14 - #15

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

## Question #15 - #17

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

## Question #16 - #18

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

## Question #17 - #19

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

## Question #18 - #20

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
