# API Gateway

API Gateway is a fully managed service provided by AWS that simplifies the process of creating, publishing, maintaining, monitoring, and securing APIs at any scale. It acts as a front door for applications to access data, business logic, or functionality from backend services, whether they are running on Amazon EC2, AWS Lambda, or on-premises systems. API Gateway handles all aspects of API management, including traffic management, authorization, monitoring, and version control, making it an essential tool for modern application development.

This documentation provides a detailed exploration of API Gateway's features, capabilities, and use cases, organized into logical sections for clarity and depth.

## Key Features of API Gateway

### Create and Manage APIs

API Gateway enables developers to create, deploy, and manage APIs with ease. It supports RESTful APIs, HTTP APIs, and WebSocket APIs, catering to a wide range of use cases. APIs can be created using the AWS Management Console, AWS CLI, or SDKs, providing flexibility in how developers interact with the service.

- **REST APIs**: Ideal for traditional RESTful applications, supporting advanced features like request/response transformations, custom authorizers, and usage plans.
- **HTTP APIs**: Designed for low-latency, cost-effective integrations with AWS Lambda and HTTP endpoints. They are lightweight and ideal for serverless architectures.
- **WebSocket APIs**: Enable real-time, bidirectional communication between clients and servers, making them suitable for applications like chat systems, real-time dashboards, and gaming platforms.

API Gateway also supports versioning, allowing developers to manage multiple versions of an API simultaneously. This ensures backward compatibility and smooth transitions when introducing new features or changes.

### Endpoint/Entry-Point for Applications

API Gateway serves as the single entry point for client applications to interact with backend services. It abstracts the complexity of managing multiple endpoints by providing a unified interface for routing requests to the appropriate backend service, whether it is hosted on AWS (e.g., Lambda, EC2, DynamoDB) or on-premises.

This centralized approach simplifies the architecture, reduces operational overhead, and ensures consistent request/response handling across all integrations.

### Sits Between Applications & Integrations (Services)

API Gateway acts as an intermediary between client applications and backend services. It handles request routing, protocol translation, and data transformation, ensuring that backend services receive requests in the correct format and that responses are returned to clients in a consistent manner.

For example, API Gateway can transform a JSON request into an XML format for a legacy backend system or modify response headers to meet client requirements. This flexibility makes it easier to integrate diverse systems and services.

### Highly Available, Scalable, and Feature-Rich

API Gateway is designed to handle high traffic volumes and scale automatically to meet demand. It provides a robust set of features to enhance API performance, security, and usability:

- **Authorization and Authentication**: Supports AWS Cognito for user authentication and Lambda authorizers for custom authorization logic.
- **Throttling**: Protects backend services from being overwhelmed by limiting the rate of incoming requests.
- **Caching**: Reduces latency and backend load by caching responses for a configurable period.
- **CORS (Cross-Origin Resource Sharing)**: Enables secure cross-origin requests for web applications.
- **Transformations**: Modifies request and response payloads to match backend or client requirements.
- **OpenAPI Specification**: Allows for easy import and export of API definitions, facilitating collaboration and integration with third-party tools.
- **Direct Integration**: Connects seamlessly with AWS services like Lambda, DynamoDB, and S3, as well as on-premises systems.

### Public Service with Private Endpoint Support

API Gateway is a public service, meaning it is accessible over the internet. However, it also supports private endpoints that are only accessible within a Virtual Private Cloud (VPC). This ensures that sensitive APIs can be securely exposed to internal resources without being publicly accessible.

### Connect to Services/Endpoints in AWS or On-Premises

API Gateway provides flexibility in connecting to backend services, whether they are hosted on AWS (e.g., Lambda, EC2, DynamoDB) or on-premises. This allows organizations to integrate existing infrastructure with modern cloud-based services, enabling hybrid architectures and seamless migration paths.

## Types of APIs Supported by API Gateway

API Gateway supports three types of APIs, each tailored to specific use cases:

### HTTP APIs

HTTP APIs are designed for low-latency, cost-effective integrations with AWS Lambda and HTTP endpoints. They are ideal for building RESTful APIs that do not require advanced API Gateway features. HTTP APIs are lightweight and optimized for serverless architectures, making them a cost-effective choice for high-performance applications.

### REST APIs

REST APIs provide a full-featured solution for building RESTful APIs. They support advanced features such as request/response transformations, custom authorizers, and usage plans. REST APIs are ideal for traditional applications that require fine-grained control over API behavior and integration with a wide range of backend services.

### WebSocket APIs

WebSocket APIs enable real-time, bidirectional communication between clients and servers. They are ideal for applications that require low-latency, two-way communication, such as chat applications, real-time dashboards, and gaming platforms. WebSocket APIs are designed to handle high volumes of concurrent connections, making them suitable for real-time use cases.

## Endpoint Types

API Gateway supports three types of endpoints, each optimized for different use cases:

### Edge-Optimized

Edge-optimized endpoints are routed through Amazon CloudFront, ensuring that requests are routed to the nearest CloudFront Point of Presence (POP). This reduces latency for globally distributed clients and is ideal for applications with a global user base.

### Regional

Regional endpoints are designed for clients located in the same AWS region as the API Gateway. This reduces latency for regional applications and is ideal for use cases where clients and backend services are located in the same region.

### Private

Private endpoints are accessible only within a VPC via an interface endpoint. This ensures that the API is only accessible to resources within the VPC, providing an additional layer of security for sensitive applications.

---

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

### AWS Cognito

AWS Cognito can be used to authenticate users and provide access tokens that can be used to authorize API requests. Cognito supports a variety of identity providers, including social identity providers such as Google and Facebook.

### Lambda Authorizer

A Lambda authorizer is a custom authorizer that uses a Lambda function to authenticate and authorize API requests. This allows you to implement custom authentication logic, such as validating API keys or checking user permissions.

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
