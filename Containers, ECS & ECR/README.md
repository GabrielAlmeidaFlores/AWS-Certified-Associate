## Elastic Container Service (ECS)

Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications. As a fully managed service, Amazon ECS comes with AWS configuration and operational best practices built-in. It's integrated with both AWS tools, such as Amazon Elastic Container Registry, and third-party tools, such as Docker. This integration makes it easier for teams to focus on building the applications, not the environment. You can run and scale your container workloads across AWS Regions in the cloud, and on-premises, without the complexity of managing a control plane.

### ECS Key Concepts

#### Container Definition

A container definition in ECS specifies the configuration for the container image, such as the image URL, port mappings, environment variables, and resource limits (CPU and memory). It outlines how the container should run within a task.

#### Task Definition

A task definition is a blueprint for running containers in ECS. It defines a set of containers that should be run together, the Docker image for each container, resource requirements, networking, logging, environment variables, and more. Essentially, it’s the description of the tasks you want to run.

#### Task Role

The task role defines the AWS Identity and Access Management (IAM) role that ECS tasks use to interact with other AWS services. It grants the necessary permissions for ECS tasks to access resources like S3, DynamoDB, or others while they run.

#### Service Definition

A service definition in ECS ensures that a specified number of task instances are running and can be distributed across EC2 instances or Fargate tasks. It manages the deployment, scaling, and updating of tasks, maintaining the desired state of tasks (e.g., maintaining a certain number of replicas).

### ECS Cluster Mode

#### EC2 Mode

In EC2 mode, you manage the infrastructure (EC2 instances) on which your containers run. When you create an ECS cluster, you launch EC2 instances that will host the containers defined in your task definitions. You are responsible for managing the lifecycle, scaling, and availability of the EC2 instances, which means more control over the underlying hardware, but also more management overhead.

In this mode, ECS schedules tasks to run on the EC2 instances in the cluster, and you can choose between EC2 launch types or use an autoscaling group to scale the number of instances in your cluster.

#### Fargate Mode

Fargate mode is a serverless option where AWS automatically manages the infrastructure needed to run containers. You don’t need to provision or manage EC2 instances yourself. Fargate takes care of provisioning compute resources, scaling, and ensuring the containers run smoothly. You simply specify the CPU and memory requirements for your tasks, and Fargate handles the rest, making it a great choice for users who prefer a fully managed container service with minimal infrastructure management.

## Elastic Kubernetes Service (EKS)

Amazon Elastic Kubernetes Service (Amazon EKS) is a fully-managed, certified Kubernetes conformant service that simplifies the process of building, securing, operating, and maintaining Kubernetes clusters on AWS. Amazon EKS integrates with core AWS services such as CloudWatch, Auto Scaling Groups, and IAM to provide a seamless experience for monitoring, scaling, and load balancing your containerized applications.

Amazon EKS provides a scalable, highly-available control plane for Kubernetes workloads. When you run applications on Amazon EKS, as with Amazon ECS, you can choose to provide the underlying compute power for your containers with Amazon EC2 instances or with AWS Fargate.

EKS Key Points:

- **AWS Managed Kubernetes**: Amazon EKS is a fully managed Kubernetes service that simplifies the deployment, management, and scaling of Kubernetes clusters. It runs open-source Kubernetes and is designed to be cloud-agnostic, ensuring compatibility with other environments and workloads. EKS automates key tasks such as patching, scaling, and cluster management, allowing you to focus on your applications and workloads rather than infrastructure maintenance.

- **Scalable Control Plane**: The control plane of EKS is highly available and scalable across multiple Availability Zones (AZs), providing fault tolerance and improved resilience. EKS automatically manages the control plane's availability and scaling, which reduces the operational burden of setting up and managing the Kubernetes control plane.

- **Integration with AWS Services**: EKS is seamlessly integrated with a variety of AWS services, including:

  - **ECR (Elastic Container Registry)**: For storing and retrieving container images used in your Kubernetes workloads.

  - **ELB (Elastic Load Balancing)**: For distributing traffic to your Kubernetes pods.

  - **IAM (Identity and Access Management)**: For controlling access to your Kubernetes resources and ensuring security with fine-grained permissions.

  - **VPC (Virtual Private Cloud)**: For securely isolating and managing network traffic between your Kubernetes resources and other AWS services.

- **Node Management**: Nodes in EKS can be managed in several ways:

  - **Self-managed nodes**: You can manually configure and manage EC2 instances as nodes in your Kubernetes cluster.

  - **Managed node groups**: EKS provides managed node groups where AWS handles the provisioning and scaling of EC2 instances, simplifying node management.

  - **Fargate**: With AWS Fargate, you can run your Kubernetes pods without needing to manage underlying EC2 instances, enabling serverless computing for Kubernetes workloads.

- **Managed VPC for Control Plane**: The EKS control plane runs in a managed AWS VPC, ensuring that your Kubernetes cluster is securely isolated and that traffic between the control plane and your worker nodes is private and protected. The managed VPC also simplifies networking setup, as it ensures proper networking configurations (e.g., routing, security groups, and subnets) are set up to work optimally with Kubernetes workloads.

## Elastic Container Registry (ECR)

An Amazon ECR private registry hosts your container images in a highly available and scalable architecture. You can use your private registry to manage private image repositories consisting of Docker and Open Container Initiative (OCI) images and artifacts. Each AWS account is provided with a default private Amazon ECR registry. For more information about Amazon ECR public registries, see Public registries in the Amazon Elastic Container Registry Public User Guide.

ECR Key Points:

- **Public and Private Registries**: Each AWS account can have both public and private registries, allowing users to decide whether to share images with the public or restrict them to specific users or services within the account. Public repositories are open for everyone to pull images, while private repositories require explicit permission for access.

- **Repositories and Images**: An ECR registry can host multiple repositories, each of which can hold multiple images. This allows for flexible organization of images according to application, version, or environment.

- **Integrated with IAM**: ECR is tightly integrated with AWS Identity and Access Management (IAM), enabling you to manage access to repositories and images at a granular level. Permissions can be granted for specific actions, such as pulling images, pushing images, or managing repositories, and can be scoped to users, roles, or services.

- **Image Scanning**: ECR supports image scanning to check for security vulnerabilities in stored images.

  - **Basic scanning**: is available by default and checks for known vulnerabilities from the Common Vulnerability and Exposure (CVE) database.

  - **Enhanced scanning**: leverages Amazon Inspector to provide deeper insights into security issues, helping to detect more subtle vulnerabilities within container images.

- **Metrics and Monitoring**: ECR provides near real-time metrics via Amazon CloudWatch, allowing you to monitor activities such as:

  - Authentication requests (failed/successful logins)

  - Push and Pull events (when images are uploaded or downloaded)

  - Repository activity (e.g., number of images, storage usage)

- **Replication**: ECR offers replication for improving availability and redundancy:

  - **Cross-region replication**: allows images to be replicated across multiple AWS regions, reducing latency for image pulls in different geographical areas and providing disaster recovery options.

  - **Cross-account replication**: allows you to automatically replicate images to other AWS accounts, which is useful in multi-account environments where different teams or applications need access to the same images.
