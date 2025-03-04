# FSx for Windows File Server

Amazon FSx for Windows File Server is a fully managed, native Windows file system service that provides shared file storage built on Windows Server. It is designed to seamlessly integrate with Windows environments, offering a robust and scalable solution for organizations that require high-performance file storage. FSx for Windows File Server eliminates the need for traditional file server administration, allowing businesses to focus on their core operations while AWS manages the underlying infrastructure.

## Key Features and Capabilities

### Fully Managed Native Windows File Servers/Shares

Amazon FSx for Windows File Server is a fully managed service, meaning that AWS handles the provisioning, patching, and maintenance of the underlying Windows Server infrastructure. This eliminates the need for organizations to manage file servers manually, reducing operational overhead and ensuring high availability and performance. The service provides native Windows file shares that are compatible with existing Windows-based applications and workflows.

### Designed for Integration with Windows Environments

FSx for Windows File Server is specifically designed to integrate with Windows environments. It supports the Server Message Block (SMB) protocol, which is the standard file-sharing protocol used by Windows. This ensures seamless compatibility with Windows-based applications, Active Directory (AD), and other Windows services. The service also supports Distributed File System (DFS), enabling organizations to create a scalable and distributed file share structure.

### Integration with Directory Service for Self-Managed AD

Amazon FSx for Windows File Server integrates with AWS Directory Service for Microsoft Active Directory, as well as self-managed Active Directory environments. This integration allows organizations to use their existing AD credentials to manage access to file shares. The service supports single or multi-AZ deployments within a Virtual Private Cloud (VPC), ensuring high availability and fault tolerance.

### Single or Multi-AZ within a VPC

FSx for Windows File Server can be deployed in either single-AZ or multi-AZ configurations. Single-AZ deployments are cost-effective and suitable for workloads that do not require high availability. Multi-AZ deployments, on the other hand, provide enhanced durability and availability by replicating data across multiple Availability Zones within a region. Both deployment options are hosted within a VPC, ensuring secure and isolated network connectivity.

### On-Demand and Scheduled Backups

The service provides robust backup capabilities, including on-demand and scheduled backups. Backups are stored in Amazon S3, offering durable and cost-effective storage. FSx for Windows File Server supports Volume Shadow Copy Service (VSS), enabling user-driven restores of previous versions of files. This feature is particularly useful for recovering from accidental deletions or modifications.

### Accessible Using VPC, Peering, VPN, and Direct Connect

Amazon FSx for Windows File Server can be accessed through various network configurations, including VPC, VPC peering, VPN, and AWS Direct Connect. This flexibility allows organizations to securely connect to their file shares from on-premises environments or other AWS regions. The service also supports enforced encryption in-transit, ensuring that data is protected during transmission.

### Native Windows File System Features

FSx for Windows File Server supports a wide range of native Windows file system features, including:

- **Data Deduplication**: Reduces storage costs by eliminating redundant data at the sub-file level.
- **Distributed File System (DFS)**: Enables the creation of a scalable and distributed file share structure.
- **KMS at-Rest Encryption**: Provides encryption for data at rest using AWS Key Management Service (KMS).
- **Enforced Encryption in-Transit**: Ensures that data is encrypted during transmission, protecting it from unauthorized access.

### Volume Shadow Copy Service (VSS) - User-Driven Restores

The service supports Volume Shadow Copy Service (VSS), allowing users to restore previous versions of files without requiring administrator intervention. This feature is particularly useful for recovering from accidental deletions or modifications, providing an additional layer of data protection.

### Native File System Accessible Over SMB

Amazon FSx for Windows File Server provides native file system access over the SMB protocol. This ensures compatibility with Windows-based applications and workflows, enabling seamless integration with existing environments. The service also supports the Windows permission model, allowing organizations to manage access to file shares using familiar tools and processes.

### Support for Distributed File System (DFS)

FSx for Windows File Server supports Distributed File System (DFS), enabling organizations to create a scalable and distributed file share structure. This feature is particularly useful for organizations with large or complex file storage requirements, as it allows for the consolidation of file shares into a single namespace.

### Managed Service - No File Server Administration

As a fully managed service, Amazon FSx for Windows File Server eliminates the need for traditional file server administration. AWS handles all aspects of the service, including provisioning, patching, and maintenance. This allows organizations to focus on their core business operations while ensuring high availability and performance.

### Integrated with Directory Service and Your Own Directory

Amazon FSx for Windows File Server integrates with AWS Directory Service for Microsoft Active Directory, as well as self-managed Active Directory environments. This integration allows organizations to use their existing AD credentials to manage access to file shares, ensuring seamless integration with existing workflows and processes.

## Deployment Options

### Single-AZ Deployment

Single-AZ deployments are cost-effective and suitable for workloads that do not require high availability. In this configuration, data is stored within a single Availability Zone, providing a balance between cost and performance. Single-AZ deployments are ideal for development and testing environments, as well as non-critical production workloads.

### Multi-AZ Deployment

Multi-AZ deployments provide enhanced durability and availability by replicating data across multiple Availability Zones within a region. This configuration ensures that data remains available even in the event of an AZ failure, making it suitable for mission-critical workloads. Multi-AZ deployments are ideal for production environments that require high availability and fault tolerance.

## Security and Compliance

### Encryption at Rest and in Transit

Amazon FSx for Windows File Server provides robust security features, including encryption at rest and in transit. Data at rest is encrypted using AWS Key Management Service (KMS), ensuring that it is protected from unauthorized access. The service also supports enforced encryption in transit, ensuring that data is encrypted during transmission.

### Windows Permission Model

FSx for Windows File Server supports the Windows permission model, allowing organizations to manage access to file shares using familiar tools and processes. This ensures that access to file shares is controlled and auditable, providing an additional layer of security.

### Compliance

Amazon FSx for Windows File Server is designed to meet a wide range of compliance requirements, including HIPAA, GDPR, and SOC. The service provides detailed audit logs and monitoring capabilities, enabling organizations to demonstrate compliance with regulatory requirements.

## Performance and Scalability

### High Performance

Amazon FSx for Windows File Server is designed to deliver high performance for a wide range of workloads. The service provides low-latency access to file shares, ensuring that applications can access data quickly and efficiently. FSx for Windows File Server also supports data deduplication, reducing storage costs and improving performance.

### Scalability

FSx for Windows File Server is highly scalable, allowing organizations to easily increase storage capacity as their needs grow. The service supports Distributed File System (DFS), enabling organizations to create a scalable and distributed file share structure. This ensures that organizations can meet their storage requirements without compromising performance.

## Backup and Restore

### On-Demand Backups

Amazon FSx for Windows File Server supports on-demand backups, allowing organizations to create backups of their file shares at any time. Backups are stored in Amazon S3, providing durable and cost-effective storage.

### Scheduled Backups

The service also supports scheduled backups, enabling organizations to automate the backup process. Scheduled backups can be configured to run daily, weekly, or monthly, ensuring that data is protected on a regular basis.

### Volume Shadow Copy Service (VSS)

FSx for Windows File Server supports Volume Shadow Copy Service (VSS), allowing users to restore previous versions of files without requiring administrator intervention. This feature is particularly useful for recovering from accidental deletions or modifications, providing an additional layer of data protection.

## Network Connectivity

### VPC

Amazon FSx for Windows File Server is hosted within a Virtual Private Cloud (VPC), ensuring secure and isolated network connectivity. This allows organizations to control access to their file shares and ensure that data is protected from unauthorized access.

### VPC Peering

The service supports VPC peering, enabling organizations to connect their VPCs and share resources across multiple AWS accounts or regions. This provides flexibility and scalability, allowing organizations to create complex network architectures.

### VPN and Direct Connect

FSx for Windows File Server can be accessed through VPN or AWS Direct Connect, enabling secure connectivity from on-premises environments. This ensures that organizations can access their file shares securely, regardless of their location.
