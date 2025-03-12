# Elastic Beanstalk

AWS Elastic Beanstalk (EB) is a Platform as a Service (PaaS) offering from Amazon Web Services (AWS) that simplifies the deployment, management, and scaling of applications. It is designed to be developer-focused, allowing developers to concentrate on writing code while EB handles the underlying infrastructure. This service is particularly beneficial for small development teams that need to deploy applications quickly without the overhead of managing servers, load balancers, or other infrastructure components.

Elastic Beanstalk is a high-level service that provides managed application environments. Users only need to provide their application code, and EB takes care of the rest, including provisioning resources, load balancing, auto-scaling, and monitoring. However, it’s important to note that while EB abstracts much of the infrastructure management, it does require some application tweaks to fully leverage its capabilities. EB is built on top of AWS CloudFormation, which means it uses CloudFormation templates to deploy and manage resources.

## Key Concepts in Elastic Beanstalk

### Elastic Beanstalk Application

An Elastic Beanstalk Application is a logical container that holds all the components related to an application. This includes not just the application code, but also the infrastructure, configurations, and versions of the application. Think of it as a folder that contains everything needed to run and manage your application.

### Application Versions

An Application Version is a specific, labeled version of the deployable code for an application. Each version is stored as a source bundle in an Amazon S3 bucket. This allows you to easily roll back to a previous version if needed. The source bundle can be a ZIP or WAR file containing your application code and any necessary configuration files.

### Environments

An Environment in Elastic Beanstalk is a collection of AWS resources that run a specific application version. Each environment is either a Web Server Tier or a Worker Tier, which determines the structure and function of the environment. For example, a Web Server Tier is designed to handle HTTP requests, while a Worker Tier is used for background processing tasks.

Each environment has its own unique CNAME (Canonical Name), which is a user-friendly URL that points to the environment. This allows you to easily access your application without needing to know the underlying IP addresses or domain names.

## Platforms Supported by Elastic Beanstalk

Elastic Beanstalk supports a wide range of platforms, making it versatile for different types of applications. The built-in platforms include:

- **Go**: A statically typed, compiled language known for its simplicity and performance.

- **Java SE**: The standard edition of Java, suitable for building desktop, server, and web applications.

- **Tomcat**: A popular open-source implementation of the Java Servlet and JavaServer Pages (JSP) technologies.

- **.NET Core (Linux) & .NET (Windows)**: Frameworks for building cross-platform and Windows-specific applications, respectively.

- **Node.js**: A JavaScript runtime built on Chrome's V8 JavaScript engine, ideal for building scalable network applications.

- **PHP**: A widely-used open-source scripting language especially suited for web development.

- **Python**: A high-level, interpreted programming language known for its readability and versatility.

- **Ruby**: A dynamic, open-source programming language with a focus on simplicity and productivity.

In addition to these, Elastic Beanstalk also supports Docker platforms, including:

- **Single Container Docker**: This mode uses EC2 instances with Docker installed, allowing you to run a single Docker container.

- **Multicontainer Docker**: This mode creates an Amazon ECS (Elastic Container Service) cluster, where multiple Docker containers can run on EC2 instances provisioned in the cluster. An Elastic Load Balancer (ELB) is also provisioned for high availability.

## Deployment Policies in Elastic Beanstalk

Elastic Beanstalk offers several deployment policies that determine how application versions are deployed to environments. Each policy has its own advantages and trade-offs, depending on the specific needs of your application.

### All at Once Deployment

In this deployment strategy, the new version of the application is deployed to all instances simultaneously. This results in a brief outage as all instances are updated at the same time. If the deployment fails, the application remains offline until the issue is resolved.

### Rolling Deployment

Rolling deployment updates instances in batches. One instance is updated at a time, while the others continue to serve traffic. This minimizes downtime but can result in a longer deployment process. If a batch fails, the deployment stops, and the remaining instances are not updated.

### Rolling with Additional Batch

This strategy is similar to the rolling deployment but adds an additional batch of instances to maintain capacity during the deployment process. This ensures that the application remains available and responsive even as instances are being updated.

### Immutable Deployment

In an immutable deployment, all new instances are created with the new version of the application. The old instances are only terminated after the new instances pass health checks. This ensures that the application remains available and that there is no downtime during the deployment.

### Traffic Splitting

Traffic splitting is similar to immutable deployment but allows you to split traffic between the old and new instances. For example, you can send 50% of the traffic to the new instances and 50% to the old instances. If the new instances pass health checks, you can gradually increase the traffic to the new instances until all traffic is routed to them. The old instances are then terminated.

## Lifecycle and RDS in Elastic Beanstalk

### RDS within Elastic Beanstalk

You can create an Amazon RDS (Relational Database Service) instance within an Elastic Beanstalk environment. This RDS instance is then linked to the EB environment, and its lifecycle is tied to the environment. If you delete the environment, the RDS instance is also deleted, which can result in data loss. Each environment has its own RDS instance, which means that different environments will have different data.

### Environment Properties for RDS

When you create an RDS instance within an EB environment, several environment properties are automatically set:

- **RDS_HOSTNAME**: The hostname of the RDS instance.
- **RDS_PORT**: The port on which the RDS instance is listening.
- **RDS_DB_NAME**: The name of the database.
- **RDS_USERNAME**: The username for accessing the database.
- **RDS_PASSWORD**: The password for accessing the database.

### Decoupling RDS from Elastic Beanstalk

To avoid data loss when deleting an environment, you can decouple the RDS instance from the EB environment. This involves creating a separate RDS instance outside of EB and configuring the environment properties to point to this external RDS instance. This way, the data is not tied to the lifecycle of the EB environment, and you can change environments without affecting the database.

To decouple an existing RDS instance from an EB environment, follow these steps:

- **Create an RDS Snapshot**: This ensures that you have a backup of your data.
- **Enable Delete Protection**: This prevents the RDS instance from being accidentally deleted.
- **Create a New EB Environment**: Create a new environment with the same application version.
- **Ensure Connectivity**: Make sure the new environment can connect to the external RDS instance.
- **Swap Environments**: Use the CNAME or DNS to swap the old environment with the new one.
- **Terminate the Old Environment**: This will attempt to delete the RDS instance, but it will fail because Delete Protection is enabled.
- **Manually Delete the Stack**: Locate the failed stack in CloudFormation and manually delete it, ensuring that the RDS instance is retained.

## Customizing Elastic Beanstalk with .ebextensions

Elastic Beanstalk allows you to customize your environment using `.ebextensions`. These are configuration files that you include in your application source bundle (ZIP or WAR file). The `.ebextensions` folder contains YAML or JSON files with a `.config` extension. These files use the CloudFormation (CFN) format to define additional resources or modify existing ones.

### Example of .ebextensions Configuration

```YAML
option_settings:
  aws:elasticbeanstalk:application:environment:
    MY_ENV_VAR: "my_value"

resources:
  myCustomResource:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "My custom security group"
      VpcId: "vpc-12345678"
```

In this example, the `option_settings` section sets an environment variable, while the `resources` section creates a new security group.

## HTTPS Configuration in Elastic Beanstalk

To secure your application with HTTPS, you need to apply an SSL certificate to the Load Balancer in your Elastic Beanstalk environment. This can be done either through the EB Console or by using `.ebextensions`.

### Steps to Configure HTTPS

- **Obtain an SSL Certificate**: You can use AWS Certificate Manager (ACM) to request a free SSL certificate.
- **Apply the SSL Certificate**: In the EB Console, navigate to the environment’s Load Balancer configuration and apply the SSL certificate.
- **Configure the Security Group**: Ensure that the security group associated with the Load Balancer allows HTTPS traffic (port 443).

### Example of HTTPS Configuration via .ebextensions

```YAML
Resources:
  sslSecurityGroupIngress:
    Type: AWS::EC2::SecurityGroupIngress
    Properties:
      GroupId: { "Ref" : "AWSEBLoadBalancerSecurityGroup" }
      IpProtocol: tcp
      FromPort: 443
      ToPort: 443
      CidrIp: 0.0.0.0/0
```

This configuration ensures that the Load Balancer allows HTTPS traffic from any IP address.

## Environment Cloning in Elastic Beanstalk

Elastic Beanstalk allows you to clone an existing environment, which is useful for creating test or staging environments that mirror your production environment. When you clone an environment, EB copies the options, environment variables, resources, and other settings. However, it’s important to note that any RDS instance included in the environment is not cloned, and no data is copied.

### Steps to Clone an Environment

- **Navigate to the EB Console**: Go to the environment you want to clone.
- **Click on "Clone Environment"**: This will create a new environment with the same configuration.
- **Configure the New Environment**: You can modify the new environment as needed, such as changing the application version or environment variables.

## Elastic Beanstalk and Docker

Elastic Beanstalk supports Docker-based applications, allowing you to deploy containerized applications with ease. There are two main modes for Docker deployments: Single Container Docker and Multicontainer Docker.

### Single Container Docker

In this mode, Elastic Beanstalk uses EC2 instances with Docker installed. You can provide one of the following:

- **Dockerfile**: EB will build a Docker image from the Dockerfile and use it to run a container.
- **Dockerrun.aws.json (version 1)**: This file specifies an existing Docker image and configures ports, volumes, and other attributes.
- **Docker-compose.yml**: If you use Docker Compose, you can provide a docker-compose.yml file to define multi-container applications.

### Multicontainer Docker

In this mode, Elastic Beanstalk creates an ECS cluster, where multiple Docker containers can run on EC2 instances. An Elastic Load Balancer (ELB) is also provisioned for high availability. To use this mode, you need to provide a `Dockerrun.aws.json` (version 2) file in the root of your application source bundle. This file defines the containers, their images, and how they should be deployed.

### Example of Dockerrun.aws.json (version 2)

```JSON
{
  "AWSEBDockerrunVersion": 2,
  "containerDefinitions": [
    {
      "name": "web",
      "image": "my-web-app:latest",
      "memory": 256,
      "portMappings": [
        {
          "hostPort": 80,
          "containerPort": 80
        }
      ]
    },
    {
      "name": "worker",
      "image": "my-worker-app:latest",
      "memory": 256
    }
  ]
}
```

This configuration defines two containers: a web container and a worker container. The web container listens on port 80, while the worker container runs in the background.

## Summary Table of Key Features

| **Feature**                         | **Description**                                                                                                                                    |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Platform as a Service (PaaS)**    | Fully managed service for deploying and scaling applications. Developers focus on code, while EB manages infrastructure.                           |
| **Developer-Focused**               | Designed for developers, not end-users. Reduces infrastructure management overhead.                                                                |
| **Managed Environments**            | Automatically handles provisioning, load balancing, auto-scaling, and monitoring.                                                                  |
| **Supported Platforms**             | Supports multiple programming languages and frameworks, including Java, .NET, Node.js, Python, Ruby, Go, PHP, and Docker (single/multi-container). |
| **Application Versions**            | Specific labeled versions of deployable code stored in S3. Allows easy rollback to previous versions.                                              |
| **Environments**                    | Web Server Tier (handles HTTP requests) or Worker Tier (background processing). Each environment has a unique CNAME.                               |
| **Deployment Policies**             | Multiple deployment strategies: All at Once, Rolling, Rolling with Additional Batch, Immutable, and Traffic Splitting.                             |
| **RDS Integration**                 | Supports Amazon RDS within EB environments. RDS lifecycle can be tied to or decoupled from the EB environment.                                     |
| **Customization via .ebextensions** | Use `.ebextensions` folder with YAML/JSON files to customize environments, add resources, or modify settings.                                      |
| **HTTPS Configuration**             | Apply SSL certificates to the Load Balancer directly via EB Console or `.ebextensions`.                                                            |
| **Environment Cloning**             | Clone existing environments to create new ones (e.g., for testing). Copies configurations but not RDS data.                                        |
| **Docker Support**                  | Supports single-container (EC2 with Docker) and multi-container (ECS cluster) deployments.                                                         |
| **Single Container Docker**         | Use Dockerfile, `Dockerrun.aws.json` (v1), or `docker-compose.yml` to deploy single-container apps.                                                |
| **Multicontainer Docker**           | Uses ECS clusters for multi-container apps. Requires `Dockerrun.aws.json` (v2) for configuration.                                                  |
| **Based on CloudFormation**         | Uses AWS CloudFormation under the hood to deploy and manage resources.                                                                             |
| **Cost**                            | No additional charge for Elastic Beanstalk itself; pay only for the underlying AWS resources used.                                                 |
| **Ideal Use Case**                  | Best suited for small to medium-sized development teams looking to deploy applications quickly with minimal infrastructure management.             |
