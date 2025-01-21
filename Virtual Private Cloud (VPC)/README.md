# AWS Virtual Private Cloud (VPC)

AWS Virtual Private Cloud (VPC) allows you to create a secure, isolated, and customizable network environment within the AWS Cloud. It provides complete control over your virtual networking resources, enabling you to define your IP address range, create subnets, configure route tables, and establish gateways for seamless and secure communication.

VPC Key Points:

- **Regional Scope**: A VPC spans all Availability Zones (AZs) in a specific AWS region, enabling seamless communication between resources across AZs using private IP addresses. This regional design ensures high availability and fault tolerance by leveraging the redundancy of multiple AZs.

- **Isolated Network**: VPCs provide a logically isolated network environment, ensuring complete separation from other VPCs and the public internet unless explicitly connected. This isolation is achieved through unique IP address ranges, routing tables, and security configurations within the VPC.

- **Strict Traffic Control**: By default, no traffic is allowed in or out of a VPC until explicitly configured. Security is enforced through security groups, which act as stateful firewalls at the instance level, and network ACLs, which provide stateless controls at the subnet level. Additional components like internet gateways, NAT gateways, and VPNs can be configured for external access.

- **Highly Flexible**: AWS VPCs are highly adaptable, supporting network architectures ranging from simple single-tier deployments to complex multi-tier designs. You can create public and private subnets, customize routing tables, and implement hybrid connectivity through VPN or AWS Direct Connect for seamless integration with on-premises networks.

- **Tenancy Options**: VPCs offer two tenancy options—Default Tenancy and Dedicated Tenancy—to meet different operational and compliance requirements.

  - **Default Tenancy**: Instances are hosted on shared hardware, providing a cost-effective solution for most workloads. Resources share the underlying physical infrastructure with other AWS customers but maintain logical isolation.

  - **Dedicated Tenancy**: Instances run on dedicated hardware isolated at the physical level. This option ensures no sharing of physical servers with other customers, offering enhanced security and compliance. Dedicated tenancy is particularly beneficial for organizations subject to strict regulatory requirements, such as financial services or healthcare.

    - **Dedicated Hosts**: Provide full control over host-level configurations, allowing placement, visibility, and usage tracking of instances.
    - **Dedicated Instances**: Instances are automatically placed on dedicated hardware but do not offer host-level control.

> [!IMPORTANT]
> When configuring a VPC, selecting dedicated tenancy at the time of creation ensures that all EC2 instances launched within the VPC are hosted on dedicated hardware. However, this choice is permanent—once a VPC is created with dedicated tenancy, you cannot change it to default tenancy later. On the other hand, if you choose default tenancy for your VPC, you retain flexibility. While the VPC itself uses shared hardware by default, you can still launch dedicated instances or use dedicated hosts for specific workloads within the VPC. This allows you to utilize dedicated hardware selectively without committing the entire VPC to dedicated tenancy.
