# FSx for Lustre

Amazon FSx for Lustre is a fully managed, high-performance file system designed to meet the demands of compute-intensive workloads such as high-performance computing (HPC), machine learning, big data analytics, and financial modeling. Built on the Lustre file system, FSx for Lustre provides a scalable, high-throughput, and low-latency solution that integrates seamlessly with AWS services, particularly Amazon S3. This documentation provides a detailed overview of FSx for Lustre, its architecture, deployment types, performance characteristics, and integration capabilities.

## Key Features and Use Cases

### High-Performance Computing (HPC) and Linux Clients (POSIX)

FSx for Lustre is optimized for workloads that require high-speed data processing and low-latency access. It adheres to the POSIX standard, making it compatible with Linux-based applications commonly used in HPC environments. This compatibility ensures that existing workflows and tools can be easily migrated to FSx for Lustre without significant modifications.

### Machine Learning, Big Data, and Financial Modeling

FSx for Lustre is particularly well-suited for machine learning training, big data analytics, and financial modeling. These workloads often involve processing large datasets and require high throughput and low latency to ensure timely results. FSx for Lustre can scale to hundreds of gigabytes per second (GB/s) in throughput and deliver sub-millisecond latency, making it ideal for these use cases.

## Deployment Types

FSx for Lustre offers two deployment types to cater to different workload requirements: Scratch and Persistent.

### Scratch Deployment

Scratch file systems are designed for short-term, high-performance workloads. They are highly optimized for speed and cost-efficiency but do not provide high availability (HA) or data replication. Scratch deployments are ideal for temporary workloads where data durability is not a primary concern.

- **Performance**: Baseline performance of 200 MB/s per TiB of storage.
- **Use Case**: Temporary workloads, such as data processing or simulation tasks that do not require long-term data retention.

### Persistent Deployment

Persistent file systems are designed for longer-term workloads that require high availability and self-healing capabilities. They provide replication within a single Availability Zone (AZ) and are suitable for workloads where data durability and reliability are critical.

- **Performance**: Offers baseline performance options of 50 MB/s, 100 MB/s, or 200 MB/s per TiB of storage.
- **Use Case**: Long-term workloads, such as machine learning model training or financial modeling, where data integrity and availability are essential.

## Performance and Scalability

FSx for Lustre is designed to deliver exceptional performance and scalability, making it suitable for the most demanding workloads.

### Baseline Performance

The baseline performance of FSx for Lustre depends on the deployment type and the size of the file system. The minimum file system size is 1.2 TiB, with increments of 2.4 TiB.

| Deployment Type | Baseline Performance         | Burst Performance        |
| --------------- | ---------------------------- | ------------------------ |
| Scratch         | 200 MB/s per TiB             | Up to 1,300 MB/s per TiB |
| Persistent      | 50, 100, or 200 MB/s per TiB | Up to 1,300 MB/s per TiB |

### Burst Performance

FSx for Lustre employs a credit-based system to allow burst performance of up to 1,300 MB/s per TiB. This feature is particularly useful for workloads with sporadic spikes in demand.

### Scalability

FSx for Lustre can scale to hundreds of GB/s in throughput, ensuring that it can handle even the most data-intensive workloads. The file system size and performance scale linearly, allowing users to tailor the system to their specific needs.

## Data Storage Architecture

FSx for Lustre employs a distributed architecture to store data efficiently. The file system splits data into two main components: metadata and data.

### Metadata Storage

Metadata, which includes information about file names, directories, and permissions, is stored on Metadata Storage Targets (MSTs). MSTs are optimized for fast access to metadata, ensuring that operations such as file lookups and directory traversals are performed quickly.

### Data Storage

File data is stored as objects on Object Storage Targets (OSTs). OSTs are responsible for storing the actual file content and are optimized for high-throughput data access. The distributed nature of OSTs allows FSx for Lustre to scale performance linearly with the size of the file system.

## Integration with Amazon S3

FSx for Lustre integrates seamlessly with Amazon S3, enabling users to leverage S3 as a data repository. This integration provides a hybrid storage solution that combines the high performance of FSx for Lustre with the durability and scalability of S3.

### Lazy Loading

When an S3 bucket is linked to an FSx for Lustre file system, data is "lazy loaded" into the file system as needed. This means that data is only transferred from S3 to FSx for Lustre when it is accessed, reducing initial data transfer times and costs.

### Data Export to S3

Data can be exported back to S3 at any point using the hsm_archive feature. This allows users to archive data to S3 for long-term storage while maintaining a high-performance file system for active workloads.

## High Availability and Self-Healing

### High Availability (HA)

Persistent file systems provide high availability within a single Availability Zone (AZ). This ensures that the file system remains accessible even in the event of hardware failures.

### Self-Healing

FSx for Lustre includes self-healing capabilities that automatically detect and recover from hardware failures. This feature minimizes downtime and ensures data integrity without requiring manual intervention.

## Backup and Retention

FSx for Lustre supports both manual and automatic backups to Amazon S3. Backups can be retained for a period of 0 to 35 days, providing flexibility in data retention policies.

### Backup Options

- **Manual Backups**: Users can initiate backups manually at any time.
- **Automatic Backups**: FSx for Lustre can be configured to perform automatic backups on a regular schedule.

### Retention Policy

Backups can be retained for up to 35 days, allowing users to restore data from a specific point in time if needed.

## Access and Connectivity

FSx for Lustre can be accessed over a Virtual Private Network (VPN) or AWS Direct Connect, ensuring secure and low-latency connectivity to the file system.

### VPN Access

VPN access provides a secure connection to FSx for Lustre over the internet, making it suitable for remote users or hybrid cloud environments.

### Direct Connect

AWS Direct Connect offers a dedicated network connection to AWS, providing lower latency and higher throughput compared to VPN. This option is ideal for workloads that require consistent and high-performance connectivity.
