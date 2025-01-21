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
  Default Communication: By default, all subnets within a VPC can communicate with each other, unless restricted by security groups, network ACLs, or route tables.

- **Subnet routing**: Each subnet must be associated with a route table, which specifies the allowed routes for outbound traffic leaving the subnet. Every subnet that you create is automatically associated with the main route table for the VPC. You can change the association, and you can change the contents of the main route table.

- **Reserved IP Addresses (5 in Total)**: Each subnet reserves five IP addresses in its CIDR range for special purposes. For example, using the subnet IP **10.16.16.0/20**:

  - **Network Address (10.16.16.0)**: The first IP address is the network address, which represents the subnet itself and cannot be used by instances.

  - **VPC Router (10.16.16.1)**: The second IP address is reserved for the VPC router. This address is used by AWS for routing traffic between subnets within the VPC and to/from other VPCs or the internet.

  - **Reserved DNS (10.16.16.2)**: The third IP address is reserved for the DNS server within the VPC. Instances can use this address for name resolution within the VPC and for external DNS lookups (if DNS support is enabled).

  - **Future Use (10.16.16.3)**: The fourth IP address is reserved for future AWS use. While it is currently unused, AWS may utilize this address for future features or network management.

  - **Broadcast Address (10.16.31.255)**: The last IP address is the broadcast address, which is the last IP in the subnet range. This address is used for broadcast communication across the subnet, though AWS does not use this feature in VPCs (unlike traditional networking). It's reserved to conform to networking standards.
