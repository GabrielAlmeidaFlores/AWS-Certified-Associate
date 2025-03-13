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
