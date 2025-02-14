## Route 53

Amazon Route 53 is a scalable and highly available Domain Name System (DNS) web service designed for managing domain names, routing internet traffic, and performing health checks. It integrates with AWS services and provides both public and private hosted zones for domain resolution.

### Hosted Zones

A Route 53 hosted zone serves as a DNS database for a domain, such as example.com. Hosted zones are authoritative for a domain and store DNS records that guide traffic to resources. They are globally resilient with multiple DNS servers and are created automatically with domain registration via Route 53, though they can also be created separately. Hosted zones store DNS records like A, AAAA, MX, NS, TXT, and more. They are used by the DNS system to resolve domain names, and Route 53 operates public name servers that are accessible over the internet. Each hosted zone is assigned four Route 53 name servers (NS) specific to the zone. Externally registered domains can also point to a Route 53 public zone. Private Hosted Zones can be created for internal networks.

#### Public Hosted Zones

Public hosted zones are used for domains accessible from the public internet and function as an authoritative DNS for a domain. The name servers (NS records) connect the hosted zone to the global DNS system, and resource records (RR) are supported for directing traffic to AWS and external services.

#### Private Hosted Zones

Private hosted zones are associated with one or more Amazon VPCs and are used for internal DNS resolution, making them inaccessible from the public internet. They can be configured with split-view DNS, allowing for overlapping public and private zones with the same domain name.

### Choosing between alias and non-alias records

Amazon Route 53 alias records provide a Route 53-specific extension to DNS functionality. Alias ​​records allow you to direct traffic to selected AWS resources, including but not limited to CloudFront distributions and Amazon S3 buckets. They also allow you to route traffic from one record in a hosted zone to another record.

Unlike a CNAME record, you cannot create an alias record at the top node of a DNS namespace, also known as the zone apex . For example, if you register the DNS name example.com, the zone apex would be example.com. You cannot create a CNAME record for example.com, but you can create an alias record for example.com that routes traffic to <www.example.com> (as long as the record type for <www.example.com> is not CNAME).

### Health Checks

Route 53 provides health checks to monitor the health and availability of endpoints. These health checks are separate from DNS records but are used for routing decisions. They are distributed globally to improve reliability, with a default check interval of 30 seconds, although 10-second checks are available at an additional cost.

You can create the following types of Amazon Route 53 health checks:

- **Health checks that monitor an endpoint**: You can configure a health check that monitors an endpoint that you specify either by IP address or by domain name. At regular intervals that you specify, Route 53 submits automated requests over the internet to your application, server, or other resource to verify that it's reachable, available, and functional. Optionally, you can configure the health check to make requests similar to those that your users make, such as requesting a web page from a specific URL.

- **Health checks that monitor other health checks (calculated health checks)**: You can create a health check that monitors whether Route 53 considers other health checks healthy or unhealthy. One situation where this might be useful is when you have multiple resources that perform the same function, such as multiple web servers, and your chief concern is whether some minimum number of your resources are healthy. You can create a health check for each resource without configuring notification for those health checks. Then you can create a health check that monitors the status of the other health checks and that notifies you only when the number of available web resources drops below a specified threshold.

- **Health checks that monitor CloudWatch alarms**: You can create CloudWatch alarms that monitor the status of CloudWatch metrics, such as the number of throttled read events for an Amazon DynamoDB database or the number of Elastic Load Balancing hosts that are considered healthy. After you create an alarm, you can create a health check that monitors the same data stream that CloudWatch monitors for the alarm.

### Routing Policies

#### Simple Routing

Simple routing lets you configure standard DNS records, with no special Route 53 routing such as weighted or latency. With simple routing, you typically route traffic to a single resource, for example, to a web server for your website.

You can use simple routing policy for records in a private hosted zone.

If you choose the simple routing policy in the Route 53 console, you can't create multiple records that have the same name and type, but you can specify multiple values in the same record, such as multiple IP addresses. (If you choose the simple routing policy for an alias record, you can specify only one AWS resource or one record in the current hosted zone.) If you specify multiple values in a record, Route 53 returns all values to the recursive resolver in random order, and the resolver returns the values to the client (such as a web browser) that submitted the DNS query. The client then chooses a value and resubmits the query. With simple routing policy, although you can specify multiple IP addresses, these IP addresses are not health checked.

#### Failover Routing

Failover routing in Amazon Route 53 is a DNS-based solution that helps ensure high availability and reliability by automatically redirecting traffic to a healthy resource when the primary resource becomes unavailable. It works by configuring two records: a primary record that points to the main resource and a secondary record that serves as a backup. When Route 53 detects that the primary resource is unhealthy, it reroutes traffic to the secondary resource, minimizing downtime and service disruptions.

Health checks play a crucial role in failover routing by continuously monitoring the availability of the primary resource. If the health check fails, Route 53 considers the primary resource unhealthy and directs traffic to the secondary resource. Health checks can monitor endpoints via HTTP, HTTPS, or TCP, and they can be integrated with CloudWatch alarms for additional monitoring capabilities. Properly configuring health checks is essential to ensuring seamless failover when failures occur.

Failover routing is commonly used for applications that require high availability, such as websites, APIs, and enterprise services. It can be implemented in active-passive or active-active configurations. In an active-passive setup, traffic flows only to the primary resource unless it fails, at which point the secondary resource takes over. In an active-active setup, multiple endpoints handle traffic, but failover ensures that only healthy resources receive requests. By leveraging Route 53's failover routing policy, businesses can enhance reliability and provide a better user experience even in the event of system failures.

#### Multivalue Answer Routing

Multivalue answer routing in Amazon Route 53 is a DNS-based traffic distribution method that allows multiple IP addresses to be associated with a single domain name. When a client queries the domain, Route 53 returns multiple IP addresses in response, helping distribute traffic across multiple resources and improving availability. This approach is particularly useful for applications that do not use load balancers but still require basic traffic distribution and fault tolerance.

Health checks play an important role in multivalue answer routing by ensuring that only healthy IP addresses are included in DNS responses. Each record can be linked to a health check, and if a resource fails, Route 53 automatically removes its corresponding IP address from the DNS response. This prevents traffic from being sent to unhealthy endpoints, reducing downtime and enhancing user experience.

Multivalue answer routing is commonly used for applications with multiple backend servers, on-premises deployments, or services running on multiple cloud instances. Unlike traditional load balancing, it does not perform intelligent traffic distribution based on metrics like latency or geography but instead provides clients with multiple IPs, allowing them to choose which one to use. This makes it a cost-effective and simple solution for improving redundancy and availability without requiring complex infrastructure.

#### Weighted Routing

Weighted routing in Amazon Route 53 is a DNS-based traffic distribution strategy that allows traffic to be distributed across multiple resources in specified proportions. Each record in Route 53 is assigned a numerical weight, and the probability of a client receiving a particular record in a DNS response is proportional to its assigned weight. This enables gradual traffic shifting, A/B testing, and load distribution across multiple environments.

One of the key advantages of weighted routing is its flexibility in directing traffic based on business needs. For example, a company can assign a higher weight to a primary server and a lower weight to a backup server, ensuring most requests go to the primary resource while keeping the backup available if needed. Similarly, during application rollouts, traffic can be incrementally shifted to a new version by adjusting the weights, allowing controlled testing and reducing the risk of failures.

Weighted routing can be combined with Route 53 health checks to improve reliability. If a resource becomes unhealthy, Route 53 automatically removes it from DNS responses, ensuring that only healthy resources receive traffic. This makes weighted routing a useful solution for managing traffic distribution, performing canary deployments, and maintaining high availability in cloud-based applications.

#### Latency-based Routing

Latency-based routing in Amazon Route 53 is a DNS routing policy that directs user requests to the AWS region with the lowest latency. When a client queries a domain, Route 53 evaluates the network latency between the user's location and multiple AWS regions, then returns the IP address of the resource that provides the fastest response time. This helps optimize performance by reducing delays and improving the user experience.

Latency-based routing is particularly useful for global applications that serve users from multiple geographical locations. By directing users to the nearest and fastest-performing region, it minimizes network lag and enhances application responsiveness. This approach is commonly used for services such as web applications, APIs, and content delivery networks that require low-latency interactions.

To ensure reliability, latency-based routing can be combined with Route 53 health checks. If a resource in the lowest-latency region becomes unhealthy, Route 53 automatically removes it from DNS responses and directs traffic to the next best-performing region. This ensures that users always connect to the most responsive and available endpoint, maintaining high availability and optimal performance for distributed applications.

#### Geolocation Routing

AWS Route 53 Geolocation Routing allows DNS queries to be answered based on the geographic location of the requestor. When a user makes a DNS request, Route 53 determines their location using the source IP address and responds with the DNS record that matches that location. You can configure records at different levels, including state (for the U.S.), country, or continent, allowing precise traffic direction. This is useful for content localization, regulatory compliance, and distributing traffic based on user location. If no matching geolocation record is found, Route 53 returns a default record if configured; otherwise, the query fails.

> [!IMPORTANT]
> Geolocation Routing does not route users to the closest server geographically. Instead, it searches for a matching record in a specific order: first, it looks for a record at the state level (if applicable), then the country level, followed by the continent level. If no record is found for the user's location, Route 53 returns the default record (if configured). This means a user in one country may not be routed to a physically closer server but rather to a server designated for their specific country or continent.

#### Geoproximity Routing

Geoproximity Routing in Amazon Route 53 allows you to route traffic based on the physical location of your resources and the geographic location of the user making the DNS request. This feature is useful when you have multiple endpoints distributed across different regions and want to direct traffic based on geographic proximity rather than just predefined geolocation rules.

Unlike Geolocation Routing, which strictly maps users to predefined regions (state, country, or continent), Geoproximity Routing considers the physical distance between users and your application’s resources. AWS determines this proximity using the geographic coordinates of AWS Regions or customer-specified locations. This enables more dynamic and efficient routing, especially for globally distributed applications.

To refine traffic flow further, you can use a bias value, which expands or shrinks the effective geographic reach of a resource. A positive bias increases the range of traffic directed to a particular endpoint, while a negative bias decreases it. This allows businesses to control traffic distribution more precisely.

Key Differences Between Geolocation and Geoproximity Routing:

| Feature             | Geolocation Routing                                     | Geoproximity Routing                          |
| ------------------- | ------------------------------------------------------- | --------------------------------------------- |
| **Routing Basis**   | Predefined geographic rules (state, country, continent) | Physical proximity of users to resources      |
| **Traffic Control** | Static assignments                                      | Dynamic, adjustable using bias values         |
| **Use Case**        | Compliance, localization                                | Performance, load balancing, and optimization |

## CloudFront

Amazon CloudFront is a global content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to users with low latency and high transfer speeds. It works seamlessly with AWS services such as Amazon S3, EC2, and Lambda@Edge, ensuring optimized performance and security. By caching content at various geographical locations, CloudFront reduces the load on origin servers and improves availability.

### CloudFront Architecture

CloudFront's architecture consists of multiple key components that work together to optimize content delivery. The primary components include the origin, distribution, regional edge caches, and edge locations.

- **Origin**: The origin is the source of the content, which can be an Amazon S3 bucket, an EC2 instance, an Elastic Load Balancer, or a custom origin such as an on-premises server. The origin stores the original version of the content and serves it to CloudFront when requested. When using a custom origin, it must be publicly accessible and properly configured to handle CloudFront requests efficiently.

- **Distribution**: The distribution is the configuration entity that tells CloudFront how to distribute content from the origin to the users. It includes settings such as caching behavior, security configurations, SSL/TLS certificates, and price classes. There are two types of distributions: web distributions for static and dynamic content and RTMP distributions for media streaming, though RTMP distributions have been deprecated in favor of HTTP-based streaming solutions.

- **Edge Locations**: Edge locations are strategically placed data centers worldwide that cache copies of content closer to end users. When a user requests content, CloudFront automatically routes the request to the nearest edge location, minimizing latency and improving load times. If the requested content is not available in the edge location, CloudFront retrieves it from a regional edge cache or directly from the origin.

- **Regional Edge Cache**: The regional edge cache acts as an intermediary layer between edge locations and the origin to further optimize performance by reducing the frequency of direct origin requests. It helps improve cache hit ratios by retaining less frequently accessed content for a longer period, reducing the number of times CloudFront must go back to the origin for content that may not be in high demand.

This image illustrates Amazon CloudFront's Content Delivery Network (CDN) and how it distributes content from origin services to end users:

<img src="https://docs.aws.amazon.com/images/whitepapers/latest/amazon-cloudfront-media/images/media-delivery-reference-architecture.png" alt="CloudFront Architecture">

### Price Classes

CloudFront Price Classes allow you to control the balance between cost and performance by selecting the regions from which CloudFront serves your content. There are three Price Classes: Price Class 100, Price Class 200, and Price Class All. Each class determines which edge locations will be used to serve your content, impacting both cost and performance.

- **Price Class 100**: Price Class 100 is the most cost-effective option. It includes only the least expensive regions for data transfer, such as edge locations in the United States, Canada, Europe, and Israel. This price class is ideal for applications with a primary audience in North America and Europe, where cost savings are prioritized over global coverage. While it offers lower data transfer costs, it may result in higher latency for users in regions not covered by this price class, such as Asia, Australia, South America, and Africa.

- **Price Class 200**: Price Class 200 includes a broader set of edge locations compared to Price Class 100, excluding only the most expensive regions. It covers all edge locations in Price Class 100, plus additional locations in Asia, Australia, and South America. This price class is suitable for applications with a global audience but where cost optimization is still important. It excludes the most expensive regions, such as the Middle East and Africa, offering a balance between cost and performance. Users in Asia, Australia, and South America will experience better latency compared to Price Class 100.

- **Price Class All**: Price Class All is the most comprehensive and expensive option. It includes all CloudFront edge locations worldwide, including the most expensive regions like the Middle East and Africa. This price class is ideal for applications with a truly global audience where performance and low latency are critical, regardless of cost. While it provides the best possible latency and performance for users in all regions, it comes with higher data transfer costs due to the inclusion of all regions.

> [!NOTE]
> You can change the Price Class for a CloudFront distribution at any time through the AWS Management Console.

### How CloudFront Caching Works

CloudFront employs a multi-tier caching system to efficiently serve content while reducing the load on the origin. When a user requests content, CloudFront first checks whether the requested object is available in the nearest edge location. If the object is cached, CloudFront immediately delivers it, ensuring low latency.

If the object is not found at the edge location, CloudFront queries a regional edge cache, which is a larger cache tier that holds recently accessed objects from multiple edge locations. If the object is found there, it is served to the user and copied to the edge location for future requests. If neither the edge location nor the regional cache has the object, CloudFront fetches it from the origin, stores it in the caches, and then delivers it to the user.

Caching behavior in CloudFront is determined by cache-control headers set by the origin. These headers dictate how long an object should be stored at edge locations before being refreshed. Additionally, CloudFront allows cache invalidation, which enables administrators to remove outdated content from the cache without waiting for the expiration time to lapse. This is particularly useful for time-sensitive content updates.

#### Manage how long content remains in the cache (expiration)

You can control how long files remain in a CloudFront cache before CloudFront forwards another request to your origin. Decreasing the duration allows you to serve dynamic content. Increasing the duration means that users get better performance because their files are more likely to be served directly from the edge cache. A longer duration also reduces the load on your origin.

Typically, CloudFront serves a file from an edge location until the specified cache duration passes, that is, until the file expires. After the cache expires, the next time the edge location receives a request for the file, CloudFront forwards the request to the origin to check whether the cache contains the latest version of the file. The origin's response depends on whether the file has changed or not:

- If the CloudFront cache already has the latest version, the origin returns a status code 304 Not Modified.
- If the CloudFront cache does not have the latest version, the origin returns a status code 200 OKand the latest version of the file.

If a file in an edge location is not requested frequently, CloudFront might delete it (remove it before its expiration date) to make room for more recently requested files.

By default, each file automatically expires after 24 hours, but you can change the default behavior in two ways:

- To change the cache duration for all files with the same path pattern, you can change the CloudFront settings for Minimum TTL , Maximum TTL , and Default TTL for a cache behavior.
- To change the cache duration for an individual file, you can configure your origin to add a header with the or Cache-Controldirective , or a header to the file.

#### Invalidate files to remove content

If you need to remove a file from CloudFront edge caches before it expires, you can do one of the following:

- Invalidate the file from edge caches. The next time a viewer requests the file, CloudFront returns to the origin to fetch the latest version of the file.
- Use file versioning to serve a different version of the file that has a different name. For example, instead of naming a graphic file `image.jpg`, you might call it `image_1.jpg`. When you want to start serving a new version of the file, you'd name the new file `image_2.jpg`, and you'd update the links in your web application or website to point to `image_2.jpg`. Alternatively, you might put all graphics in an `images_v1` directory and, when you want to start serving new versions of one or more graphics, you'd create a new `images_v2` directory, and you'd update your links to point to that directory. With versioning, you don't have to wait for an object to expire before CloudFront begins to serve a new version of it, and you don't have to pay for object invalidation.

> [!NOTE]
> To control file versions in your distribution, you can either invalidate files or use versioned file names. However, versioning is recommended for frequent updates as it ensures users receive the correct file despite caching, simplifies log analysis, enables serving different versions to different users, facilitates rolling updates, and is more cost-effective by avoiding invalidation fees.

### CloudFront Distributions

An Amazon CloudFront distribution is the core configuration unit that defines how content is delivered from your origin servers to end-users. It acts as a global content delivery network (CDN) that caches content at edge locations, reducing latency and improving performance for users worldwide. When creating a CloudFront distribution, you must configure various settings to optimize content delivery, enhance security, and meet specific application requirements.

#### Content Origin

The origin is the source of the content that CloudFront delivers to users. A single CloudFront distribution can support up to 25 origins, allowing you to serve content from multiple sources. Origins can be of different types, each suited for specific use cases:

- **Amazon S3 Buckets**: S3 is the most common origin for static content such as images, videos, and documents. When using S3 as an origin, you can configure it as a website endpoint or a REST API endpoint. To secure access to the S3 bucket, you can use Origin Access Control (OAC) or Origin Access Identity (OAI), which restrict direct access to the bucket and ensure that all requests are routed through CloudFront.

- **Custom HTTP Servers**: Custom origins are used for dynamic content or APIs hosted on HTTP servers. These servers can be Amazon EC2 instances, Elastic Load Balancers (ELB), or even on-premises servers. When using a custom origin, you must ensure that the server is properly configured to handle requests from CloudFront, including setting up security groups, firewall rules, and SSL/TLS certificates.

- **AWS Media Services**: For media delivery, you can use AWS Elemental MediaPackage for video streaming or AWS Elemental MediaStore for media storage and delivery. These services are optimized for high-performance media workloads and integrate seamlessly with CloudFront.

- **Other AWS Services**: CloudFront can also integrate with Lambda Function URLs for serverless compute or API Gateway for RESTful APIs. These integrations enable you to deliver dynamic content or serverless applications at scale.

#### Access Control

Access control in CloudFront determines how users interact with your content. You can configure your distribution to allow public access or restrict access to specific users or groups:

- **Public Access**: By default, CloudFront serves content to all users without restrictions. This is ideal for publicly available content such as websites, blogs, or media files.

- **Restricted Access**: For private content, you can use signed URLs or signed cookies to restrict access. Signed URLs are time-limited URLs that grant access to specific files, while signed cookies provide access to multiple files for a limited time. Additionally, you can use AWS Identity and Access Management (IAM) policies to restrict access to specific AWS accounts or roles.

#### Security

Security is a critical aspect of CloudFront configurations. CloudFront provides several features to protect your content and ensure secure delivery:

- **AWS WAF (Web Application Firewall)**: AWS WAF integrates with CloudFront to protect against common web exploits such as SQL injection, cross-site scripting (XSS), and distributed denial-of-service (DDoS) attacks. You can configure WAF rules to block malicious traffic or allow only trusted sources.

- **HTTPS Enforcement**: CloudFront supports HTTPS using AWS Certificate Manager (ACM) or custom SSL/TLS certificates. You can enforce HTTPS-only access to ensure that all communication between users and CloudFront is encrypted.

- **Field-Level Encryption**: For sensitive data such as credit card numbers or personal information, you can enable field-level encryption. This feature encrypts specific fields in HTTP requests at the edge before forwarding them to the origin, ensuring end-to-end security.

#### Cache Key Customization

The cache key is a unique identifier that CloudFront uses to store and retrieve cached content. By customizing the cache key, you can control how CloudFront caches your content and improve cache efficiency:

- **Query String Parameters**: You can configure CloudFront to include or exclude specific query string parameters in the cache key. For example, if your application uses query strings for language settings (e.g., `?lang=en`), you can cache content separately for each language.

- **HTTP Headers**: CloudFront can cache content based on specific HTTP headers such as `Accept-Language` or `User-Agent`. This is useful for serving different content to users based on their device type or language preferences.

- **Cookies**: You can configure CloudFront to include or exclude specific cookies in the cache key. For example, if your application uses cookies for session management, you can cache content separately for each session.

#### Origin Request Settings

Origin request settings define how CloudFront interacts with your origin server. These settings allow you to customize the requests that CloudFront sends to the origin:

- **Forward Headers**: You can configure CloudFront to include specific headers (e.g., `Authorization`) in requests to the origin. This is useful for passing authentication tokens or other metadata to the origin.

- **Forward Cookies**: CloudFront can forward cookies to the origin for personalized content. For example, if your application uses cookies to track user preferences, you can forward these cookies to the origin to serve customized content.

- **Forward Query Strings**: CloudFront can include query strings in requests to the origin. This is useful for dynamic content that depends on query string parameters.

#### Geographic Restrictions

CloudFront allows you to restrict access to your content based on the geographic location of your users. This feature is useful for complying with regional regulations or licensing agreements:

- **Geo-Blocking**: You can block users from specific countries using AWS WAF or CloudFront's built-in geo-restriction feature. For example, you can block access to your content from countries where it is not licensed.

- **Geo-Whitelisting**: Alternatively, you can allow access only from selected regions. This is useful for restricting access to specific markets or regions.

#### Logging

Logging is essential for monitoring and analyzing content delivery. CloudFront provides two types of logs:

- **Standard Logs**: Standard logs track user activity and request details. These logs are stored in an S3 bucket and can be analyzed using tools like Amazon Athena or AWS Glue.
- **Real-Time Logs**: Real-time logs provide near-instant visibility into traffic patterns. These logs are useful for troubleshooting and monitoring live traffic.

### Cache Behaviors

Cache behaviors define how CloudFront handles requests for specific content paths. Each cache behavior associates a path pattern (e.g., `/images/*.jpg`) with specific settings and an origin. This allows you to customize content delivery and caching based on your application's requirements.

#### Path Pattern

The path pattern specifies the URL paths to which the cache behavior applies. For example:

- `/images/*.jpg` : Targets all `.jpg` files within the images directory and its subdirectories.

- `/api/*`: Targets all requests to the api path.

- `*`: Acts as the default cache behavior, which is mandatory in every CloudFront distribution. This behavior applies when no other path patterns match a request.

- **Priority Order** : If multiple behaviors could match a request, CloudFront evaluates them in priority order, from most specific to least specific. For example, a request for `/img/cats/kitten.jpg` will match `/img/cats/*` before `/img/*`.

#### Target Origin

The target origin defines the origin server from which CloudFront retrieves content when a request matches the path pattern. Supported origins include:

- **Amazon S3 Buckets**: Ideal for static content.
- **Custom HTTP Servers**: For dynamic content or APIs.
- **AWS Media Services**: For media delivery.

#### Viewer Protocol Policy

The viewer protocol policy determines the protocols (HTTP and/or HTTPS) that viewers can use to access your content. Options include:

- **Allow All**: Viewers can use both HTTP and HTTPS.
- **Redirect to HTTPS**: HTTP requests are automatically redirected to HTTPS.
- **HTTPS Only**: Only HTTPS requests are allowed; HTTP requests receive a 403 Forbidden response.

#### Allowed HTTP Methods

The allowed HTTP methods setting specifies which HTTP methods CloudFront processes and forwards to your origin. Options include:

- **GET, HEAD**: For retrieving content.
- **POST, PUT, DELETE**: For modifying content.
- **OPTIONS, PATCH**: For advanced use cases.

#### Cache Policy

The cache policy defines how CloudFront caches content. Key settings include:

- **Minimum, Maximum, and Default TTL**: Control how long content is cached.
- **Cache Key Settings**: Include or exclude headers, cookies, and query strings.

#### Origin Request Policy

The origin request policy controls the information (headers, cookies, query strings) that CloudFront includes in requests to your origin. Key settings include:

- **Forward Headers**: Include specific headers (e.g., `Authorization`).
- **Forward Cookies**: Pass cookies to the origin.
- **Forward Query Strings**: Include query strings in requests.

#### Function Associations

Function associations allow you to associate CloudFront Functions or Lambda@Edge functions with specific cache behaviors. These functions execute custom code in response to viewer or origin requests. Use cases include:

- **Authentication**: Validate user credentials before serving content.
- **URL Rewriting**: Modify URLs dynamically.
- **A/B Testing**: Serve different content based on user attributes.

#### Response Headers Policy

The response headers policy enables you to add or modify HTTP response headers before CloudFront returns the response to the viewer. Use cases include:

- **Security Headers**: Add headers like `Content-Security-Policy` or `Strict-Transport-Security`.
- **CORS Headers**: Enable cross-origin resource sharing.
