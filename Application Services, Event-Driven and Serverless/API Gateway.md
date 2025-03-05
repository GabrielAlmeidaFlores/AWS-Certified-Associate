# API Gateway

API Gateway is a fully managed service provided by AWS that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. It acts as a front door for applications to access data, business logic, or functionality from your backend services, such as applications running on Amazon EC2, code running on AWS Lambda, or any web application. API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management.

API Gateway supports containerized and serverless workloads, as well as web applications. It provides a highly available, scalable, and cost-effective way to manage APIs, ensuring that your applications can handle the demands of your users without requiring you to manage the underlying infrastructure.

## Key Features of API Gateway

### Create and Manage APIs

API Gateway allows developers to create RESTful APIs and WebSocket APIs that enable real-time two-way communication applications. APIs can be created using the AWS Management Console, AWS CLI, or SDKs. Once created, APIs can be managed through the same interfaces, allowing for updates, monitoring, and version control.

### Endpoint/Entry-Point for Applications

API Gateway serves as the entry point for applications to interact with backend services. It provides a single, unified endpoint that routes incoming requests to the appropriate backend service, whether it is hosted on AWS or on-premises. This simplifies the architecture and reduces the complexity of managing multiple endpoints.

### Sits Between Applications & Integrations (Services)

API Gateway acts as an intermediary between client applications and backend services. It handles request routing, composition, and protocol translation, ensuring that the backend services receive requests in the correct format and that the responses are returned to the client in a consistent manner.

### Highly Available, Scalable, and Feature-Rich

API Gateway is designed to be highly available and scalable, automatically scaling to handle traffic spikes without requiring manual intervention. It includes features such as:

- **Authorization and Authentication**: Supports AWS Cognito and Lambda authorizers for secure access control.
- **Throttling**: Limits the rate of requests to protect backend services from being overwhelmed.
- **Caching**: Reduces the number of calls made to backend integrations by caching responses.
- **CORS (Cross-Origin Resource Sharing)**: Enables secure cross-origin requests.
- **Transformations**: Modifies requests and responses to match the expected format.
- **OpenAPI Specification**: Allows for easy import and export of API definitions.
- **Direct Integration**: Connects directly to AWS services such as Lambda, DynamoDB, and S3.

### Public Service

API Gateway is a public service, meaning it is accessible over the internet. However, it also supports private endpoints that are only accessible within a Virtual Private Cloud (VPC).

### Connect to Services/Endpoints in AWS or On-Premises

API Gateway can connect to backend services hosted on AWS, such as Lambda functions, EC2 instances, or on-premises servers. This flexibility allows organizations to integrate existing infrastructure with modern cloud-based services.

## Types of APIs Supported by API Gateway

API Gateway supports three types of APIs, each designed for specific use cases:

- **HTTP APIs**: HTTP APIs are designed for low-latency, cost-effective integrations with AWS Lambda and HTTP endpoints. They are ideal for building RESTful APIs that do not require advanced API Gateway features.

- **REST APIs**: REST APIs provide a full-featured solution for building RESTful APIs. They support advanced features such as request/response transformations, custom authorizers, and usage plans.

- **WebSocket APIs**: WebSocket APIs enable real-time, two-way communication between clients and servers. They are ideal for applications that require low-latency, bidirectional communication, such as chat applications or real-time dashboards.

## Endpoint Types

API Gateway supports three types of endpoints, each optimized for different use cases:

- **Edge-Optimized**: Edge-optimized endpoints are routed through Amazon CloudFront, ensuring that requests are routed to the nearest CloudFront Point of Presence (POP). This reduces latency for globally distributed clients.

- **Regional**: Regional endpoints are designed for clients that are located in the same AWS region as the API Gateway. This reduces latency for regional applications and is ideal for use cases where clients and backend services are located in the same region.

- **Private**: Private endpoints are accessible only within a VPC via an interface endpoint. This ensures that the API is only accessible to resources within the VPC, providing an additional layer of security.

## Stages and Deployments

API Gateway uses stages to manage different versions of an API. Each stage represents a unique URL that can be used to access the API. Stages are associated with a deployment, which is a snapshot of the API configuration at a specific point in time.

### Canary Deployments

API Gateway supports canary deployments, which allow you to deploy new versions of an API to a subset of users before rolling it out to all users. This enables you to test new features or changes in a controlled environment. Canary deployments can be configured to send a specific percentage of traffic to the new version, and this percentage can be adjusted over time. Once the new version is stable, it can be promoted to become the base stage.

## Monitoring and Logging

API Gateway integrates with Amazon CloudWatch to provide detailed monitoring and logging capabilities. CloudWatch can store and manage full-stage request and response logs, as well as metrics for both the client and integration sides. This allows you to monitor the performance of your APIs and troubleshoot issues as they arise.

### CloudWatch Metrics

API Gateway provides a variety of CloudWatch metrics, including:

- **4XX Errors**: Client-side errors, such as invalid requests.
- **5XX Errors**: Server-side errors, such as backend service failures.
- **Latency**: The time taken for API Gateway to process requests.
- **Request Count**: The number of requests received by the API.

## Caching

API Gateway includes a built-in caching feature that can be used to reduce the number of calls made to backend integrations. This improves client performance and reduces the load on backend services.

### Cache Configuration

- **TTL (Time to Live)**: The default TTL for cached responses is 300 seconds, but this can be configured to any value between 0 and 3600 seconds.
- **Cache Size**: The cache size can range from 500MB to 237GB, depending on your requirements.
- **Encryption**: Cached responses can be encrypted to ensure data security.

## Authentication and Authorization

API Gateway provides several options for securing your APIs, including:

- **AWS Cognito**: AWS Cognito can be used to authenticate users and provide access tokens that can be used to authorize API requests. Cognito supports a variety of identity providers, including social identity providers such as Google and Facebook.

- **Lambda Authorizer**: A Lambda authorizer is a custom authorizer that uses a Lambda function to authenticate and authorize API requests. This allows you to implement custom authentication logic, such as validating API keys or checking user permissions.

## Error Handling

API Gateway provides detailed error codes to help you troubleshoot issues with your APIs. These error codes are divided into two categories:

### 4XX Errors (Client Errors)

- **400 Bad Request**: The request was malformed or invalid.
- **403 Access Denied**: The request was denied by an authorizer or filtered by AWS WAF.
- **429 Too Many Requests**: The request was throttled due to exceeding the rate limit.

### 5XX Errors (Server Errors)

- **502 Bad Gateway**: The backend service returned an invalid response.
- **503 Service Unavailable**: The backend service is offline or experiencing major issues.
- **504 Integration Failure/Timeout**: The backend service did not respond within the 29-second timeout limit.

## Summary Table of Key Features

| **Feature**                 | **Description**                                                                         |
| --------------------------- | --------------------------------------------------------------------------------------- |
| **API Types Supported**     | HTTP APIs, REST APIs, WebSocket APIs                                                    |
| **Endpoint Types**          | Edge-Optimized, Regional, Private                                                       |
| **Scalability**             | Automatically scales to handle hundreds of thousands of concurrent API calls            |
| **High Availability**       | Built-in redundancy and failover mechanisms for uninterrupted service                   |
| **Traffic Management**      | Throttling, caching, and request/response transformations                               |
| **Caching**                 | Configurable TTL (0–3600 seconds), cache size (500MB–237GB), and encryption support     |
| **Authentication**          | AWS Cognito, Lambda authorizers, and custom authentication mechanisms                   |
| **Monitoring & Logging**    | Integration with CloudWatch for metrics, logs, and alarms                               |
| **Error Handling**          | Detailed HTTP error codes (4XX for client errors, 5XX for server errors)                |
| **Canary Deployments**      | Gradual rollouts of new API versions with adjustable traffic percentages                |
| **Direct Integrations**     | Connects to AWS services (Lambda, EC2, DynamoDB, etc.) and on-premises systems          |
| **CORS Support**            | Enables secure cross-origin resource sharing for web applications                       |
| **OpenAPI Specification**   | Import and export API definitions using OpenAPI (formerly Swagger)                      |
| **Private Endpoints**       | Accessible only within a VPC via interface endpoints for enhanced security              |
| **Real-Time Communication** | WebSocket APIs for low-latency, bidirectional communication                             |
| **Cost-Effective**          | Pay-as-you-go pricing model with no upfront costs                                       |
| **Global Reach**            | Edge-optimized endpoints leverage CloudFront for low-latency global access              |
| **Security**                | Integration with AWS WAF for protection against common web exploits                     |
| **Transformations**         | Modify request/response payloads to match backend or client requirements                |
| **Stages & Deployments**    | Organize APIs into stages (e.g., dev, prod) and manage deployments with version control |
