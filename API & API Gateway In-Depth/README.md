# API Gateway

Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the "front door" for applications to access data, business logic, or functionality from your backend services. Using API Gateway, you can create RESTful APIs and WebSocket APIs that enable real-time two-way communication applications. API Gateway supports containerized and serverless workloads, as well as web applications.

## Resources & Methods

API Gateway provides a structured way to manage APIs by defining resources and methods. A resource represents an endpoint (such as /users or /orders), while a method (such as GET, POST, or DELETE) defines the allowed operations on that resource. Each method can have specific configurations, including authorization, request validation, and backend integration. API Gateway allows fine-grained control over access and behavior for each resource and method.

### Integration Types

Integration types determine how API Gateway interacts with backend services. Each integration type has specific use cases and configurations that allow API Gateway to forward requests, process responses, and handle errors efficiently.

#### Mock Integration

A Mock Integration is used when an API does not require a backend service. API Gateway itself generates the response based on a preconfigured response template. This integration is useful for testing, returning static responses, or handling placeholder responses when the backend is under development.

#### Lambda Integration

Lambda Integration allows API Gateway to invoke an AWS Lambda function to process API requests. This integration is commonly used for serverless applications, where business logic is executed in Lambda functions without the need for a dedicated backend server. The response from the Lambda function is returned to the client via API Gateway. This type of integration supports both proxy and non-proxy configurations, depending on whether API Gateway should handle request transformation.

#### HTTP Integration

HTTP Integration enables API Gateway to forward API requests to an external HTTP or HTTPS endpoint. This is useful for integrating with existing web services, REST APIs, or third-party APIs. The API Gateway can act as a reverse proxy, applying security controls, rate limiting, and authentication before forwarding the request.

#### AWS Service Integration

AWS Service Integration allows API Gateway to directly call AWS services, such as AWS Step Functions, Amazon S3, DynamoDB, or SNS. This eliminates the need for an intermediate application layer and enables seamless interactions with AWS services using API Gateway. The integration requires properly configuring IAM permissions to authorize API Gateway to interact with the target AWS service.

#### VPC Links

VPC Links provide a way to securely connect API Gateway to private resources within an Amazon Virtual Private Cloud (VPC), such as EC2 instances, private ALBs (Application Load Balancers), or internal services. This allows API Gateway to route traffic securely to backend services that are not publicly accessible.

## Mapping Templates

Mapping templates are used to transform incoming requests before forwarding them to the backend and to modify responses before sending them to the client. API Gateway uses Velocity Template Language (VTL) for these transformations. Mapping templates allow customization of request payloads, request headers, query parameters, and response formatting. They are essential for adapting API requests to match the expected format of backend services.

## Deployment & Stages

API Gateway supports staging and deployment management, allowing developers to deploy and test different versions of an API. Each deployment is assigned a stage, such as dev, staging, or production, which allows teams to test and release changes gradually. Stages help manage API lifecycle efficiently, enabling versioning, rollback, and controlled releases.

Here are some common stage configuration options:

| Feature              | Description                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| Stage Variables      | Key-value pairs used to manage environment-specific settings.          |
| Logging & Monitoring | Can enable CloudWatch Logs, Metrics, and X-Ray Tracing for debugging.  |
| Throttling           | Controls the rate of API requests per stage.                           |
| Caching              | Improves performance by caching responses for a configurable duration. |

### Canary Deployments

Canary Deployments enable gradual rollout of API changes by sending a small percentage of traffic to a new version while keeping the majority of traffic on the stable version. This reduces the risk of breaking the API by allowing real-time monitoring before fully deploying changes. Canary deployments also help in A/B testing of API versions.

### Throttling & Rate Limits

Throttling and rate limiting control how many API requests a client can make within a given time period. This prevents overuse, protects backend services, and ensures fair distribution of resources. API Gateway provides different levels of throttling to safeguard APIs from excessive traffic.

#### Throttling Settings

API Gateway supports two main types of throttling:

- Per-client throttling: Limits requests per API key or IAM user.
- Global throttling: Sets a general limit on API requests to prevent system overload.

These settings include:

- Rate limit: The maximum number of requests per second.
- Burst limit: The maximum number of requests allowed in a short burst.

#### Usage Plans & API Keys

Usage plans define access control by assigning API clients specific request limits. API Gateway allows API keys to be issued to clients and mapped to usage plans. Features include:

- **API Key-based access**: Clients authenticate using API keys.
- **Custom quota settings**: Define daily, weekly, or monthly request limits.
- **Multiple plans**: Create different tiers (e.g., free, premium) with varying rate limits.

Usage plans help monetize APIs, enforce fair access, and prevent service abuse.
