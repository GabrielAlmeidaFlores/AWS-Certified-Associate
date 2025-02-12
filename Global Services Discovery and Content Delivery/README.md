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
