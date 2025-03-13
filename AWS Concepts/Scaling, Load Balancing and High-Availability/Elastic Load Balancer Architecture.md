# Elastic Load Balancer Architecture

Elastic Load Balancer (ELB) is a fully managed service provided by AWS that automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. It ensures high availability, fault tolerance, and scalability for applications. This documentation provides a comprehensive overview of the ELB architecture, focusing on its key components, configurations, and operational details.

## Load Balancers Types

Amazon Web Services (AWS) offers three types of load balancers to distribute incoming traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses. These load balancers ensure high availability, fault tolerance, and scalability for applications. The three types are the Application Load Balancer (ALB), the Network Load Balancer (NLB), and the Classic Load Balancer (CLB). Each type operates at different layers of the OSI model and is designed to address specific use cases, ranging from modern microservices architectures to high-performance, low-latency applications. Below is a detailed comparison of these load balancer types in a structured table format.

| **Load Balancer Type**              | **Key Features**                                                                                                                                                    | **Use Cases**                                                                                                                  | **Protocols Supported** | **OSI Layer** |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------- | ------------- |
| **Application Load Balancer (ALB)** | Content-based routing (host/path-based), Supports WebSockets and HTTP/2, SSL/TLS termination, Advanced health checks, Integration with containers and microservices | Modern web applications, Microservices architectures, Applications requiring advanced routing (e.g., path-based or host-based) | HTTP, HTTPS, WebSockets | Layer 7       |
| **Network Load Balancer (NLB)**     | Ultra-high performance and low latency, Static IP support, Source IP preservation, TLS termination, Cross-zone load balancing                                       | High-performance applications (e.g., gaming, real-time streaming), TCP/UDP-based applications, Use cases requiring static IPs  | TCP, UDP, TLS           | Layer 4       |
| **Classic Load Balancer (CLB)**     | Basic load balancing, SSL/TLS termination, Sticky sessions, Health checks, Supports both Layer 4 and Layer 7                                                        | Legacy applications, Simple load balancing needs, Applications not requiring advanced features                                 | HTTP, HTTPS, TCP, SSL   | Layers 4 & 7  |

## Load Balancer Nodes

When you provision an Elastic Load Balancer, AWS places one or more Load Balancer nodes into the specified subnets. These nodes are the fundamental components responsible for distributing incoming traffic to the registered targets.

### Functionality of Load Balancer Nodes

Load Balancer nodes act as intermediaries between clients and the backend targets (e.g., EC2 instances). They receive incoming traffic, evaluate the listener rules, and forward the traffic to the appropriate targets based on the configured routing algorithms. Each node operates independently, ensuring redundancy and high availability. If one node fails, the remaining nodes continue to handle traffic.

Routing Algorithms:

| Routing Method        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Load Balancer Type                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| **Round-Robin**       | The Load Balancer distributes requests sequentially across all available targets. This method ensures an even distribution of traffic when all targets have equal processing capacity. It is a simple and effective approach for balancing loads in environments where the processing power of each target is approximately the same.                                                                                                                                                                                                                                                                                                                                          | Used by **Application Load Balancer (ALB)** and **Classic Load Balancer (CLB) for HTTP/HTTPS**. |
| **Least Connections** | The Load Balancer forwards requests to the target with the fewest active connections. This approach helps balance the load efficiently when targets have varying processing power or different session durations. It is particularly useful in applications where some requests require longer processing times than others.                                                                                                                                                                                                                                                                                                                                                   | Used by **Classic Load Balancer (CLB) for TCP listeners**.                                      |
| **Flow Hash Routing** | Flow Hash Routing works by using a hash function to distribute network traffic across multiple paths in a deterministic way. First, a flow is identified by its unique attributes, such as source/destination IP addresses and ports. These attributes are fed into a hash function, which generates a fixed hash value. This value is then used to select a specific path from the available options, ensuring all packets from the same flow follow the same route. This approach not only balances traffic evenly across paths but also preserves packet order within each flow, which is crucial for maintaining reliable communication, especially for protocols like TCP | Used by **Network Load Balancer (NLB)**.                                                        |

## Scalability and Availability

AWS automatically scales the number of Load Balancer nodes based on the incoming traffic volume. This ensures that the load balancer can handle increased demand without manual intervention. The nodes are distributed across multiple Availability Zones (AZs) within a region, providing fault tolerance and resilience against AZ failures.

## DNS Record Creation

When a Load Balancer is created, AWS automatically generates a single DNS record of type A (Address record). This DNS record resolves to the IP addresses of the Load Balancer nodes.

### How DNS Resolution Works

The DNS record provided by AWS is a fully qualified domain name (FQDN) that points to the Load Balancer nodes. Clients interact with the Load Balancer using this DNS name, and the DNS service resolves the name to the IP addresses of the nodes. This abstraction allows AWS to manage the underlying infrastructure without requiring clients to update their configurations.

## Internet-Facing vs. Internal Load Balancers

Elastic Load Balancers can be configured as either Internet-facing or Internal, depending on the use case.

| Internet-Facing Load Balancer                                                                                                                                                                                                                                                                             | Internal Load Balancer                                                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| An Internet-facing Load Balancer is designed to handle traffic from the public internet. Each Load Balancer node in an Internet-facing configuration is assigned a public IP address. Clients from the internet can access the application by resolving the Load Balancer's DNS name to these public IPs. | An Internal Load Balancer is used for applications that require traffic to remain within a private network, such as internal microservices or database layers. The nodes of an Internal Load Balancer do not receive public IP addresses. Instead, they use private IPs, ensuring that traffic does not traverse the public internet. |

## Target Configuration and Subnet Requirements

### Target Types

Load Balancers can route traffic to various target types, including EC2 instances, containers, and IP addresses. Notably, EC2 instances do not need to have public IP addresses to work with a Load Balancer. This allows for secure architectures where backend instances remain private, and only the Load Balancer is exposed to the internet.

### Subnet Requirements

For a Load Balancer to function correctly, each specified subnet must meet the following requirements:

- **Minimum Free IPs**: At least 8 free IP addresses per subnet.

- **Subnet Size**: A subnet with a CIDR block of /27 or larger.

These requirements ensure that the Load Balancer has sufficient IP addresses to scale and accommodate additional nodes as traffic increases.

## Listener Configuration

Listeners are a critical component of the Load Balancer architecture. They define how the Load Balancer handles incoming traffic.

### Listener Functionality

A listener is a process that checks for connection requests from clients. It is configured with:

- **Protocol**: The protocol used for incoming traffic (e.g., HTTP, HTTPS, TCP).
- **Port**: The port on which the Load Balancer listens for traffic.
- **Target Protocol and Port**: The protocol and port used to communicate with the backend targets.

For example, a listener can be configured to accept HTTPS traffic on port 443 and forward it to HTTP traffic on port 80 of the backend instances.

### Listener Rules

Listeners can be further customized with rules that determine how traffic is routed. Rules can be based on conditions such as:

- Host headers (for HTTP/HTTPS).
- Path patterns (for HTTP/HTTPS).
- Source IP addresses.

## Cross-Zone Load Balancing

Cross-Zone Load Balancing is a feature that allows a Load Balancer to distribute traffic evenly across all registered targets in all enabled Availability Zones (AZs), regardless of the AZ in which the target is located.

### How Cross-Zone Load Balancing Works

If Cross-Zone Load Balancing is turned off, the Load Balancer only distributes traffic to targets in the same AZ as the Load Balancer node receiving the traffic. However, when Cross-Zone Load Balancing is enabled, traffic is distributed evenly across all targets in all AZs. This ensures optimal utilization of resources and prevents uneven load distribution.

> [!NOTE]
> Cross-Zone Load Balancing is enabled by default for Application Load Balancers (ALBs) but disabled by default for Network Load Balancers (NLBs). For Classic Load Balancers (CLBs), the default behavior varies depending on the AWS region.
