# Application Load Balancer (ALB) and Network Load Balancer (NLB)

This documentation provides a detailed comparison and explanation of Amazon Web Services (AWS) Application Load Balancer (ALB) and Network Load Balancer (NLB). The content is structured to ensure clarity and depth, with tables used to present key differences in an organized manner. All information is based on official AWS documentation to ensure accuracy and reliability.

## Application Load Balancer (ALB)

An Application Load Balancer (ALB) operates at Layer 7 (Application Layer) of the OSI model. It is designed to handle HTTP and HTTPS traffic, making it ideal for web applications. ALBs provide advanced routing capabilities, content-based load balancing, and support for modern application architectures such as microservices.

### Key Features

- **Layer 7 Load Balancer**: ALB listens exclusively to HTTP and HTTPS traffic. It does not support other Layer 7 protocols such as SMTP, SSH, or gaming protocols. Additionally, it does not support raw TCP or UDP listeners.

- **Content-Based Routing**: ALB can route traffic based on various attributes, including:

  - **Host-header**: Route based on the domain name.
  - **Path-pattern**: Route based on the URL path.
  - **HTTP-header**: Route based on specific HTTP headers.
  - **HTTP-request-method**: Route based on the HTTP method (GET, POST, etc.).
  - **Query-string**: Route based on query parameters.
  - **Source-IP**: Route based on the client's IP address.

- **SSL/TLS Termination**: ALB always terminates SSL/TLS connections for HTTPS traffic. This means the ALB decrypts the traffic and establishes a new connection to the backend application. ALB requires SSL certificates to be installed if HTTPS is used.

- **Health Checks**: ALB performs Layer 7 health checks, which evaluate the health of the application by sending HTTP/HTTPS requests and analyzing the responses. This ensures that traffic is only routed to healthy instances.

- **Rules and Actions**: ALB uses rules to determine how incoming requests are processed. Rules are evaluated in priority order, with a default rule acting as a catch-all. Actions include:

  - **Forward**: Send traffic to a target group.
  - **Redirect**: Redirect traffic to a different URL.
  - **Fixed-response**: Return a custom HTTP response.
  - **Authenticate-oidc**: Integrate with OpenID Connect for authentication.
  - **Authenticate-cognito**: Integrate with AWS Cognito for authentication.

- **Performance**: ALB is slower compared to NLB because it processes more layers of the network stack. However, it provides richer functionality for HTTP/HTTPS traffic.

## Network Load Balancer (NLB)

The Network Load Balancer (NLB) operates at Layer 4 of the OSI model, handling TCP, UDP, and TLS traffic. It is designed for high-performance, low-latency use cases, making it suitable for applications such as gaming servers, financial systems, and other non-HTTP workloads.

### Key Features

- **Layer 4 Load Balancer**: NLB supports TCP, TLS, UDP, and TCP_UDP protocols. It has no visibility into HTTP or HTTPS traffic, meaning it cannot inspect headers, cookies, or other Layer 7 content.

- **High Performance**: NLB is extremely fast, capable of handling millions of requests per second (rps) with latency as low as 25% of ALB's latency. This makes it ideal for latency-sensitive applications.

- **Unbroken Encryption**: Unlike ALB, NLB does not terminate TLS connections. Instead, it forwards encrypted traffic directly to the backend servers, maintaining end-to-end encryption.

- **Static IP Addresses**: NLB can be assigned static IP addresses, which is useful for whitelisting in security configurations.

- **Health Checks**: NLB performs Layer 4 health checks, which only verify the ability to establish a TCP handshake or ICMP ping. It does not evaluate application-level health.

- **Use Cases**: NLB is ideal for non-HTTP workloads such as SMTP, SSH, gaming servers, and financial applications. It is also used with AWS PrivateLink to provide services to other VPCs.

## Choosing Between ALB and NLB

When deciding between ALB and NLB, consider the following factors:

- **Traffic Type**: If your application uses HTTP or HTTPS, ALB is the preferred choice due to its advanced routing and content-based features. For non-HTTP traffic (e.g., gaming, SMTP, SSH), NLB is more suitable.

- **Performance Requirements**: If your application requires ultra-low latency and high throughput, NLB is the better option. ALB, while slower, provides richer functionality for HTTP/HTTPS traffic.

- **SSL/TLS Handling**: If you need to terminate SSL/TLS at the load balancer, use ALB. If you require end-to-end encryption, use NLB.

- **Static IP Addresses**: If your use case requires static IP addresses for whitelisting or other purposes, NLB is the only option.

- **Health Checks**: If you need application-aware health checks, ALB is the better choice. For basic connectivity checks, NLB suffices.

- **PrivateLink Integration**: If you are using AWS PrivateLink to expose services to other VPCs, NLB is required.

The following table summarizes the key differences between ALB and NLB to help users choose the appropriate load balancer for their use case:

| Feature                   | Application Load Balancer (ALB)        | Network Load Balancer (NLB)                  |
| ------------------------- | -------------------------------------- | -------------------------------------------- |
| **OSI Layer**             | Layer 7 (HTTP/HTTPS)                   | Layer 4 (TCP, UDP, TLS)                      |
| **Protocol Support**      | HTTP, HTTPS                            | TCP, UDP, TLS, TCP_UDP                       |
| **SSL/TLS Termination**   | Yes (terminates SSL/TLS)               | No (forwards encrypted traffic)              |
| **Static IP Addresses**   | No                                     | Yes                                          |
| **Performance**           | Slower (processes more network layers) | Faster (millions of rps, 25% of ALB latency) |
| **Health Checks**         | Layer 7 (application-aware)            | Layer 4 (TCP/ICMP handshake)                 |
| **Use Cases**             | HTTP/HTTPS applications, microservices | Non-HTTP workloads (gaming, SMTP, SSH, etc.) |
| **Content-Based Routing** | Yes (host-header, path-pattern, etc.)  | No                                           |
| **Session Stickiness**    | Yes (based on cookies)                 | No                                           |
| **PrivateLink Support**   | No                                     | Yes                                          |
