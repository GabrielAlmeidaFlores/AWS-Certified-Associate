# Elastic File System (EFS)

Amazon Elastic File System (EFS) is a fully managed, scalable, and cloud-based file storage service provided by Amazon Web Services (AWS). It is designed to provide shared file storage for use with Amazon EC2 instances and other AWS services. EFS is built on the Network File System version 4 (NFSv4) protocol, making it compatible with Linux-based applications and workloads. This documentation provides a detailed exploration of EFS, covering its architecture, features, performance modes, storage classes, and best practices for implementation.

## Key Features of Amazon EFS

### NFSv4 Implementation

EFS is an implementation of the NFSv4 protocol, which is a widely used standard for file sharing in Linux environments. This ensures compatibility with a broad range of Linux-based applications and tools. The NFSv4 protocol provides enhanced security, performance, and stateful operations compared to earlier versions, making it ideal for cloud-based file storage.

### Linux Compatibility

EFS is designed exclusively for Linux-based systems. It can be mounted on Linux EC2 instances, allowing multiple instances to access the same file system simultaneously. This shared access is particularly useful for applications that require concurrent access to data, such as content management systems, data analytics platforms, and development environments.

### Shared Access Across EC2 Instances

One of the primary advantages of EFS is its ability to be shared across multiple EC2 instances. This shared file system enables collaborative workflows and ensures consistency across distributed applications. For example, a web application running on multiple EC2 instances can use EFS to store and access shared configuration files, logs, or user uploads.

### Private Service via Mount Targets

EFS operates as a private service within a Virtual Private Cloud (VPC). Access to the file system is provided through Mount Targets, which are network endpoints associated with specific subnets in the VPC. Each Mount Target has a private IP address, allowing EC2 instances within the VPC to securely connect to the file system.

## Mount Targets in EFS

### Definition and Role

Mount Targets are essential components of EFS that enable EC2 instances to access the file system. Each Mount Target is associated with a specific subnet and Availability Zone (AZ) within a VPC. When an EC2 instance mounts an EFS file system, it connects to the Mount Target in its subnet.

### High Availability Best Practices

To ensure high availability and fault tolerance, it is recommended to create Mount Targets in multiple subnets across different Availability Zones. This setup ensures that even if one subnet or AZ becomes unavailable, the file system remains accessible through other Mount Targets. For example, in a VPC with three subnets (Subnet A, Subnet B, and Subnet C), creating Mount Targets in all three subnets provides redundancy and improves reliability.

### Access from On-Premises Environments

EFS can also be accessed from on-premises environments using AWS Direct Connect (DX) or a Virtual Private Network (VPN). This capability allows hybrid cloud architectures, where on-premises servers and EC2 instances can share the same file system.

## EFS and VPC Integration

### VPC-Only Operation

EFS operates entirely within a VPC, ensuring that file system access is restricted to authorized resources. This private network configuration enhances security by preventing unauthorized access from the public internet.

### POSIX Permissions

EFS uses POSIX (Portable Operating System Interface) permissions to manage access to files and directories. POSIX permissions allow fine-grained control over read, write, and execute permissions for users and groups. This feature ensures that EFS integrates seamlessly with Linux-based applications that rely on standard file system permissions.

## Performance Modes in EFS

EFS offers two performance modes to cater to different workload requirements:

- **General Purpose Mode**: General Purpose mode is the default performance mode and is suitable for the majority of use cases (99.9%). It provides low latency and is ideal for applications such as web serving, content management, and home directories.
- **MAX I/O Mode**: MAX I/O mode is designed for workloads that require higher levels of throughput and can tolerate increased latency. This mode is optimized for data-intensive applications such as big data analytics, media processing, and scientific computing.

| Performance Mode | Latency | Throughput | Use Cases                             |
| ---------------- | ------- | ---------- | ------------------------------------- |
| General Purpose  | Low     | Moderate   | Web serving, content management       |
| MAX I/O          | Higher  | High       | Big data, media processing, analytics |

## Throughput Modes in EFS

EFS provides two throughput modes to optimize performance based on workload requirements:

- **Bursting Throughput Mode**: Bursting Throughput mode is the default option and is suitable for workloads with variable or unpredictable traffic patterns. In this mode, EFS automatically scales throughput based on the size of the file system. Larger file systems receive higher baseline throughput and can burst to higher levels for short periods.
- **Provisioned Throughput Mode**: Provisioned Throughput mode allows users to specify a fixed level of throughput independent of the file system size. This mode is ideal for applications with consistent, high-throughput requirements, such as video editing or large-scale data processing.

| Throughput Mode | Scalability | Use Cases                                 |
| --------------- | ----------- | ----------------------------------------- |
| Bursting        | Automatic   | Variable workloads, unpredictable traffic |
| Provisioned     | Fixed       | Consistent high-throughput workloads      |

## Storage Classes in EFS

EFS offers two storage classes to optimize cost and performance:

- **Standard Storage Class**: The Standard storage class is designed for frequently accessed data. It provides low-latency access and is suitable for active workloads such as application logs, user uploads, and databases.
- **Infrequent Access (IA) Storage Class**: The Infrequent Access (IA) storage class is cost-effective for data that is accessed less frequently. It is ideal for long-term storage of backups, archives, and other data that does not require immediate access.

| Storage Class          | Access Frequency | Cost Efficiency | Use Cases                            |
| ---------------------- | ---------------- | --------------- | ------------------------------------ |
| Standard               | Frequent         | Moderate        | Active workloads, application data   |
| Infrequent Access (IA) | Rare             | High            | Backups, archives, long-term storage |

## Lifecycle Management in EFS

EFS supports lifecycle policies to automate the transition of files between storage classes. For example, a lifecycle policy can be configured to move files that have not been accessed for 30 days from the Standard storage class to the Infrequent Access (IA) storage class. This feature helps optimize storage costs without manual intervention.

## Security and Access Control

### Encryption

EFS supports encryption at rest and in transit. Data stored in EFS can be encrypted using AWS Key Management Service (KMS), ensuring that sensitive information is protected. Additionally, data transferred between EC2 instances and EFS is encrypted using industry-standard TLS protocols.

### Network Security

Access to EFS is restricted to the VPC by default. Security groups can be used to control inbound and outbound traffic to Mount Targets, providing an additional layer of network security.
