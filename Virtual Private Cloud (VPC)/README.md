# AWS Virtual Private Cloud (VPC)

AWS Virtual Private Cloud (VPC) allows you to create a secure, isolated, and customizable network environment within the AWS Cloud. It provides complete control over your virtual networking resources, enabling you to define your IP address range, create subnets, configure route tables, and establish gateways for seamless and secure communication.

VPC Key Points:

- **Regional Scope**: A VPC spans all Availability Zones (AZs) in a specific AWS region, enabling seamless communication between resources across AZs using private IP addresses. This regional design ensures high availability and fault tolerance by leveraging the redundancy of multiple AZs.

- **Isolated Network**: VPCs provide a logically isolated network environment, ensuring complete separation from other VPCs and the public internet unless explicitly connected. This isolation is achieved through unique IP address ranges, routing tables, and security configurations within the VPC.

- **Strict Traffic Control**: By default, no traffic is allowed in or out of a VPC until explicitly configured. Security is enforced through security groups, which act as stateful firewalls at the instance level, and network ACLs, which provide stateless controls at the subnet level. Additional components like internet gateways, NAT gateways, and VPNs can be configured for external access.

- **Highly Flexible**: AWS VPCs are highly adaptable, supporting network architectures ranging from simple single-tier deployments to complex multi-tier designs. You can create public and private subnets, customize routing tables, and implement hybrid connectivity through VPN or AWS Direct Connect for seamless integration with on-premises networks.

- **Primary Private IPv4 CIDR Block**: Every VPC must have one primary private IPv4 CIDR block defined during its creation. This CIDR block specifies the range of private IP addresses available for resources within the VPC. The size of the CIDR block must be between /28 (16 IP addresses) and /16 (65,536 IP addresses). The primary CIDR block is a mandatory configuration and cannot be removed, but additional secondary CIDR blocks can be added later to expand the address space if needed.

- **DNS Settings**

  - **enableDnsSupport**: The `enableDnsSupport` attribute, when enabled (default setting), allows instances in the VPC to resolve public domain names using AWS DNS servers. This feature is essential for workloads that need to access external services, such as internet-based APIs or other AWS services. If disabled, DNS resolution within the VPC is restricted, and resources cannot resolve domain names to IP addresses. You cannot disable `enableDnsSupport` once the VPC is created, as it is enabled by default for all VPCs.

  - **enableDnsHostnames**: The `enableDnsHostnames` attribute, when enabled, assigns public DNS hostnames to instances with public IP addresses. This setting is particularly useful for accessing instances over the internet using easily recognizable names instead of IP addresses. By default, this attribute is disabled in custom VPCs and enabled in the default VPC. For `enableDnsHostnames` to work, `enableDnsSupport` must also be enabled. If needed, you can enable `enableDnsHostnames` at any time via the AWS Management Console, CLI, or SDKs by modifying the VPC settings.

- **Tenancy Options**: VPCs offer two tenancy options—Default Tenancy and Dedicated Tenancy—to meet different operational and compliance requirements.

  - **Default Tenancy**: Instances are hosted on shared hardware, providing a cost-effective solution for most workloads. Resources share the underlying physical infrastructure with other AWS customers but maintain logical isolation.

  - **Dedicated Tenancy**: Instances run on dedicated hardware isolated at the physical level. This option ensures no sharing of physical servers with other customers, offering enhanced security and compliance. Dedicated tenancy is particularly beneficial for organizations subject to strict regulatory requirements, such as financial services or healthcare.

    - **Dedicated Hosts**: Provide full control over host-level configurations, allowing placement, visibility, and usage tracking of instances.
    - **Dedicated Instances**: Instances are automatically placed on dedicated hardware but do not offer host-level control.

> [!IMPORTANT]
> When configuring a VPC, selecting dedicated tenancy at the time of creation ensures that all EC2 instances launched within the VPC are hosted on dedicated hardware. However, this choice is permanent—once a VPC is created with dedicated tenancy, you cannot change it to default tenancy later. On the other hand, if you choose default tenancy for your VPC, you retain flexibility. While the VPC itself uses shared hardware by default, you can still launch dedicated instances or use dedicated hosts for specific workloads within the VPC. This allows you to utilize dedicated hardware selectively without committing the entire VPC to dedicated tenancy.

## Subnets

A subnet is a range of IP addresses in your VPC. You launch AWS resources, such as Amazon EC2 instances, into your subnets. You can connect a subnet to the internet, other VPCs, and your own data centers, and route traffic to and from your subnets using route tables.

Subnet Key Concepts:

- **AZ Resilient**: Subnets are designed to be deployed within a specific Availability Zone (AZ), providing fault tolerance and high availability. You can create subnets across multiple AZs to ensure resilience.

- **Subnetwork of a VPC**: A subnet is a subdivision of a VPC and is associated with a single Availability Zone (AZ). Each subnet can contain multiple resources, like EC2 instances, within the selected AZ.

- **Single AZ**: Each subnet is tied to a single AZ, but an AZ can contain multiple subnets. This allows for high availability and redundancy across different AZs in the same region.

- **IPv4 CIDR Block**: Each subnet is assigned a CIDR block that is a subset of the VPC's CIDR block. The subnet CIDR block defines the range of IP addresses available within that subnet.

- **No Overlap**: The CIDR block of a subnet cannot overlap with other subnets in the same VPC. Proper planning is essential to avoid IP conflicts.

- **Default Communication**: By default, all subnets within a VPC can communicate with each other, unless restricted by security groups, network ACLs, or route tables.

- **Subnet routing**: Each subnet must be associated with a route table, which specifies the allowed routes for outbound traffic leaving the subnet. Every subnet that you create is automatically associated with the main route table for the VPC. You can change the association, and you can change the contents of the main route table.

- **Reserved IP Addresses (5 in Total)**: Each subnet reserves five IP addresses in its CIDR range for special purposes. For example, using the subnet IP **10.16.16.0/20**:

  - **Network Address (10.16.16.0)**: The first IP address is the network address, which represents the subnet itself and cannot be used by instances.

  - **VPC Router (10.16.16.1)**: The second IP address is reserved for the VPC router. This address is used by AWS for routing traffic between subnets within the VPC and to/from other VPCs or the internet.

  - **Reserved DNS (10.16.16.2)**: The third IP address is reserved for the DNS server within the VPC. Instances can use this address for name resolution within the VPC and for external DNS lookups (if DNS support is enabled).

  - **Future Use (10.16.16.3)**: The fourth IP address is reserved for future AWS use. While it is currently unused, AWS may utilize this address for future features or network management.

  - **Broadcast Address (10.16.31.255)**: The last IP address is the broadcast address, which is the last IP in the subnet range. This address is used for broadcast communication across the subnet, though AWS does not use this feature in VPCs (unlike traditional networking). It's reserved to conform to networking standards.

## Internet Gateway (IGW)

An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.

An internet gateway enables resources in your public subnets (such as EC2 instances) to connect to the internet if the resource has a public IPv4 address or an IPv6 address. Similarly, resources on the internet can initiate a connection to resources in your subnet using the public IPv4 address or IPv6 address. For example, an internet gateway enables you to connect to an EC2 instance in AWS using your local computer.

An internet gateway provides a target in your VPC route tables for internet-routable traffic. For communication using IPv4, the internet gateway also performs network address translation (NAT).

Key Features of Internet Gateway:

- **High Availability**: Designed to be horizontally scalable and redundant, ensuring uninterrupted service. Operates across multiple Availability Zones within a region.

- **Exclusive Attachment**: Can only be attached to one VPC at a time. This exclusivity ensures that no overlapping traffic occurs between VPCs.

- **Network Address Translation (NAT)**: Performs NAT for IPv4 traffic to map private IPs in the VPC to public IPs for internet communication. Does not require NAT functionality for IPv6 traffic as it is inherently internet-routable.

- **AWS Public Zone Integration**: Provides connectivity to AWS public services like Amazon S3, SQS, SNS, and others via internet-routable endpoints. Supports secure access to these services when configured with security groups and network ACLs.

- **Cost-Efficiency**: There is no additional cost for using an IGW itself. You only pay for the associated data transfer out of the VPC and other internet-based AWS services.

## Network Access Control List (NACL)

A network access control list (ACL) allows or denies specific inbound or outbound traffic at the subnet level. You can use the default network ACL for your VPC, or you can create a custom network ACL for your VPC with rules that are similar to the rules for your security groups in order to add an additional layer of security to your VPC.

NACL Key Points:

- **Default Network ACL**: Your VPC automatically comes with a modifiable default network ACL. By default, it allows all inbound and outbound IPv4 traffic and, if applicable, IPv6 traffic.

- **Custom Network ACLs**: You can create a custom network ACL and associate it with a subnet to allow or deny specific inbound or outbound traffic at the subnet level.

- **Subnet Association with Network ACLs**: Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.

- **One-to-Many Association**: You can associate a network ACL with multiple subnets. However, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.

- **Inbound and Outbound Rules**: A network ACL has inbound rules and outbound rules. Each rule can either allow or deny traffic. Each rule has a number from 1 to 32766. Rules are evaluated in order, starting with the lowest numbered rule, when deciding whether to allow or deny traffic. If the traffic matches a rule, the rule is applied, and no further rules are evaluated. It is recommended to create rules in increments (e.g., increments of 10 or 100) to allow for inserting new rules later if needed.

- **Traffic Evaluation Scope**: Network ACL rules are evaluated when traffic enters and leaves the subnet, not as it is routed within a subnet.

- **Stateless Nature of Network ACLs**: Network ACLs are stateless, meaning they do not retain information about previously sent or received traffic. For example, if you create a rule to allow specific inbound traffic to a subnet, responses to that traffic are not automatically allowed. This differs from security groups, which are stateful and automatically allow responses to inbound traffic regardless of outbound rules.

## Security Groups

A security group controls the inbound and outbound traffic for the resources it is associated with. For example, when you associate a security group with an EC2 instance, it determines the traffic allowed to reach and leave the instance.

The rules of a security group specify the inbound traffic permitted to access resources and the outbound traffic allowed to leave them. You can configure these rules by defining the source, port range, and protocol for inbound traffic, as well as the destination, port range, and protocol for outbound traffic.

When you create a VPC, it comes with a default security group. You can also create additional security groups within a VPC, each with its own customizable rules to meet specific security requirements.

Security group basics:

- **VPC and Resource Association**: You can assign a security group only to resources created within the same VPC as the security group. Additionally, a resource can have multiple security groups assigned to it.

- **Stateful Nature of Security Groups**: Security groups are stateful, meaning that response traffic is automatically allowed regardless of explicit rules. For instance, if an instance sends a request, the response traffic is permitted to reach the instance even if there are no inbound rules allowing it. Similarly, responses to allowed inbound traffic are permitted to leave the instance, regardless of outbound rules.

- **Allow Rules Only**: You can configure security groups to include rules that explicitly allow specific types of traffic. However, security groups do not support rules for explicitly denying traffic; any traffic not explicitly allowed is automatically denied by default.

- **Default Inbound Rule**: When you first create a security group, it has no inbound rules. As a result, no inbound traffic is allowed until you manually add inbound rules.

- **Default Outbound Rules**: A newly created security group includes a default outbound rule that allows all outbound traffic from the resource. You can modify this by removing the default rule and adding specific outbound rules. If there are no outbound rules, all outbound traffic is denied.

- **Aggregation of Rules**: When multiple security groups are associated with a resource, the rules from each group are aggregated into a single set. This combined set of rules determines whether to allow or deny traffic to and from the resource.
