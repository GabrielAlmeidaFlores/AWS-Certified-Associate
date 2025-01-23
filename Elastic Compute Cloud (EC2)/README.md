# Elastic Compute Cloud (EC2)

O Amazon Elastic Compute Cloud (Amazon EC2) oferece uma capacidade de computação escalável sob demanda na Nuvem Amazon Web Services (AWS). O uso do Amazon EC2 reduz os custos de hardware para que você possa desenvolver e implantar aplicações com mais rapidez. É possível usar o Amazon EC2 para executar quantos servidores virtuais forem necessários, configurar a segurança e as redes e gerenciar o armazenamento. Você pode adicionar capacidade (aumentar a escala verticalmente) para lidar com tarefas de computação pesada, como processos mensais ou anuais ou picos no tráfego do site. Quando o uso diminui, você pode reduzir a capacidade (reduzir a escala verticalmente) de novo.

Uma instância do EC2 é um servidor virtual na Nuvem AWS. Quando executa uma instância do EC2, o tipo de instância que você especifica determina o hardware disponível para sua instância.

## EC2 Instance Types

When you launch an instance, the instance type that you specify determines the hardware of the host computer used for your instance. Each instance type offers different compute, memory, and storage capabilities, and is grouped in an instance family based on these capabilities. Select an instance type based on the requirements of the application or software that you plan to run on your instance.

| **Instance Family**       | **Instance Type**      | **Use Case**                                                                      |
| ------------------------- | ---------------------- | --------------------------------------------------------------------------------- |
| **General Purpose**       | t4g, t3, t3a, t2       | Balanced compute, memory, and networking. Ideal for small to medium apps.         |
|                           | m6g, m5, m5a, m5n, m4  | For workloads that require a balance of compute, memory, and networking.          |
| **Compute Optimized**     | c7g, c6g, c6i, c5n, c5 | High-performance compute for applications like batch processing and gaming.       |
| **Memory Optimized**      | r6g, r5, r5a, r5n, r4  | Memory-intensive workloads like high-performance databases and analytics.         |
|                           | x2idn, x2iezn          | Extreme memory requirements, such as in-memory databases and big data.            |
| **Accelerated Computing** | inf1, f1, dl1          | Specialized hardware acceleration for ML inference and custom computing tasks.    |
| **Storage Optimized**     | i4i, i3, i3en          | High I/O operations for workloads like NoSQL databases and data warehousing.      |
|                           | d2, h1                 | For large-scale data processing and data lakes requiring high storage throughput. |

## EC2 Storage Concepts

### Block Storage vs. File Storage vs. Object Storage

Block, object, and cloud file storage are three ways of storing data in the cloud so that users and applications can access it remotely over a network connection. Object storage stores and manages all data in an unstructured format and in units called objects. Block storage takes any data, like a file or database entry, and divides it into blocks of equal sizes. It then stores the data block on underlying physical storage in a way that’s optimized for fast access and retrieval. Cloud file storage is another data storage method that provides servers and applications access to data through shared file systems. Each type offers its own unique advantages for various use cases.

Key differences between object storage, block storage, and file storage:

| **Aspect**              | **Block Storage**                                                                                    | **File Storage**                                                                                                   | **Object Storage**                                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| **Definition**          | Stores data in fixed-sized blocks, accessible at the raw disk level.                                 | Organizes data as files within a hierarchical directory structure.                                                 | Stores data as objects, each containing data, metadata, and a unique identifier.                          |
| **Access Method**       | Low-level, block-by-block access. Requires a filesystem for readability (e.g., NTFS, ext4).          | File-level access with a high-level interface (e.g., NFS, SMB).                                                    | Accessed via APIs (e.g., REST, S3 API), not a traditional filesystem.                                     |
| **File Management**     | Can store files but requires additional management resources and budget to handle files effectively. | Supports common file-level protocols and permissions models. Works with applications that use shared file storage. | Files are stored as objects, requiring API-driven applications or new code for direct access.             |
| **Metadata Management** | Uses minimal metadata for block operations.                                                          | Stores limited metadata relevant to files only.                                                                    | Can store unlimited metadata for each object, allowing custom fields for specific use cases.              |
| **Performance**         | High-performance, low latency, and rapid data transfer.                                              | Offers high performance for shared file access and collaborative workflows.                                        | Stores unlimited data with minimal latency; slightly higher latency than block storage for small objects. |
| **Physical Storage**    | Distributed across SSDs or HDDs in on-premises or cloud setups.                                      | On-premises NAS servers or underlying physical block storage.                                                      | Distributed across multiple storage nodes, providing high durability and redundancy.                      |
| **Use Cases**           | Databases, virtual machines, OS boot drives, high-performance apps.                                  | Shared file storage, backups, collaborative tools, and applications requiring hierarchical access.                 | Cloud-native applications, backups, media storage, IoT data, analytics, and storing unstructured data.    |
| **Bootable**            | Yes, because it allows direct, raw access for bootloaders and OS installation.                       | No, as it lacks raw access for booting.                                                                            | No, designed for data storage rather than booting.                                                        |
| **Scalability**         | Scales with storage volumes (block devices); somewhat limited compared to object storage.            | Scales for user or application file access; somewhat limited compared to object storage.                           | Unlimited scalability, designed for massive amounts of unstructured data.                                 |

Key Takeaway:

- **Block Storage**: Best for low-level access, OS booting, databases, and performance-critical applications where low latency is required.
- **File Storage**: Ideal for collaborative workflows, shared access, hierarchical file structures, and applications using common file protocols.
- **Object Storage**: Perfect for scalable, cloud-native applications, storing large amounts of unstructured data (e.g., photos, videos, backups), and metadata-rich use cases.

### Storage Performance Concepts

Storage performance is a critical factor in determining how effectively a system can handle data-intensive workloads. Whether you're working with databases, virtual machines, or media files, understanding key metrics such as IO (Block) Size, IOPS, and Throughput is essential for optimizing performance and selecting the right storage solution.

Each of these concepts plays a unique role in how storage systems process and transfer data, influencing factors like speed, efficiency, and scalability.

#### IO (Block) Size

IO Size refers to the amount of data read from or written to the storage system in a single Input/Output (IO) operation. It represents how much data is handled per IO request. The size can vary from a few kilobytes (KB) to several megabytes (MB), depending on the task at hand. Smaller block sizes are best for random access tasks, while larger blocks are more efficient for sequential data processing.

For instance, in a database environment like MySQL, transactions typically involve many small read and write operations, each in the range of 4KB to 8KB. This is due to the random access pattern of database operations, where specific rows or records need to be accessed frequently. On the other hand, when transferring large files such as videos or backups, using larger block sizes like 64KB or 1MB is more efficient. The reason is that large blocks can be read or written in a single operation, reducing overhead and optimizing the transfer speed, especially when dealing with large, sequential data.

#### IOPS (Input/Output Operations Per Second)

IOPS measures how many individual read or write operations a storage system can perform in one second. This is an important metric for systems with high-frequency, random access tasks such as databases, virtual machines, or any application that performs numerous small data operations. A higher IOPS count indicates better performance for workloads requiring frequent, small read and write operations.

Consider an online transaction processing (OLTP) system, where thousands of customers make purchases every second. This system demands storage with high IOPS, as it has to handle many small, random read and write requests. For example, a system capable of 100,000 IOPS would efficiently support this environment by quickly processing each individual request. In contrast, older hard disk drives (HDDs) typically handle only around 100-200 IOPS, which would not be sufficient for high-demand systems, resulting in slow performance and potential delays.

#### Throughput

Throughput refers to the total amount of data that a storage system can transfer per second, typically measured in megabytes per second (MB/s) or gigabytes per second (GB/s). This metric is crucial for tasks that involve large, sequential data transfers, such as media rendering, video editing, or backing up large volumes of data. High throughput allows data to flow efficiently from the source to the storage system, minimizing bottlenecks.

Throughput is typically calculated as the IO (Block) Size multiplied by the IOPS (Input/Output Operations Per Second). This means that throughput is directly dependent on both the size of each data block being transferred and the number of IO operations the system can handle per second.

For example, if you're transferring a 10GB video file to a storage server, and the system's block size is 64KB with an IOPS of 1,000, the throughput would be:

_Throughput = IO (Block) Size × IOPS -> 64KB × 1,000 = 64MB/s_

With a throughput of 64MB/s, the transfer time for the 10GB file would be approximately 156.25 seconds (10GB ÷ 64MB/s).

However, if the system can handle 5000 IOPS with the same block size (64KB), the throughput increases to:

_Throughput = 64KB × 5,000 = 320MB/s_

In this case, the transfer of the same 10GB file would only take approximately 31.25 seconds.

In high-throughput environments, where bulk data transfer is frequent—such as in video production or large-scale backups—the system's throughput, which is the product of IO (Block) Size and IOPS, becomes a critical factor in determining overall performance.

## EC2 Storage Types

Amazon EC2 provides you with flexible, cost effective, and easy-to-use data storage options for your instances. Each option has a unique combination of performance and durability. These storage options can be used independently or in combination to suit your requirements.

### Elastic Block Storage

- **Block Storage**: EBS provides raw disk allocations (volumes) that can be encrypted using AWS Key Management Service (KMS) for security.

- **Instance-Level Storage**: Volumes are attached to EC2 instances or other services, where a file system (such as ext3, ext4, or XFS) can be created on the device.

- **Availability Zone (AZ)**: EBS storage is provisioned within a specific AZ, offering resilience within that AZ, but not across multiple AZs by default.

- **Attach & Detach Flexibility**: Volumes are attached to a single EC2 instance (or other supported services) over a storage network. EBS volumes can be detached from one instance and re-attached to another, with the data persisting throughout.

- **Snapshots & Backup**: EBS volumes can be backed up to Amazon S3 as snapshots. Snapshots can be used to create new volumes and migrate data between AZs.

- **Multiple Storage Options**: EBS provides various physical storage types, sizes, and performance profiles to suit different workloads (e.g., General Purpose SSD, Provisioned IOPS SSD).

- **Billing**: EBS is billed based on GB per month, with additional charges for performance (IOPS, throughput) depending on the selected volume type.

#### Solid state drive (SSD) volumes

SSD-backed volumes are optimized for transactional workloads involving frequent read/write operations with small I/O size, where the dominant performance attribute is IOPS. SSD-backed volume types include General Purpose SSD and Provisioned IOPS SSD . The following is a summary of the use cases and characteristics of SSD-backed volumes.

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="2" align="center" style="text-align: center;">Amazon EBS General Purpose SSD volumes</th>
      <th colspan="2" align="center" style="text-align: center;">Amazon EBS Provisioned IOPS SSD volumes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td tabindex="-1"><b>Volume type</b></td>
      <td tabindex="-1"><code>gp3</code></td>
      <td tabindex="-1"><code>gp2</code></td>
      <td tabindex="-1"><code>io2</code> Block Express <sup>3</sup></td>
      <td tabindex="-1"><code>io1</code></td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Durability</b></td>
      <td colspan="2" align="center" style="text-align: center;" tabindex="-1">99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
      <td tabindex="-1">99.999% durability (0.001% annual failure rate)</td>
      <td tabindex="-1">99.8% - 99.9% durability (0.1% - 0.2% annual failure rate)</td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Use cases</b></td>
      <td colspan="2" tabindex="-1">
        <ul>
          <li>Transactional workloads</li>
          <li>Virtual desktops</li>
          <li>Medium-sized, single-instance databases</li>
          <li>Low-latency interactive applications</li>
          <li>Boot volumes</li>
          <li>Development and test environments</li>
        </ul>
      </td>
      <td tabindex="-1">
        <p>Workloads that require:</p>
        <ul>
          <li>Sub-millisecond latency</li>
          <li>Sustained IOPS performance</li>
          <li>More than 64,000 IOPS or 1,000 MiB/s of throughput</li>
        </ul>
      </td>
      <td tabindex="-1">
        <ul>
          <li>Workloads that require sustained IOPS performance or more than 16,000 IOPS</li>
          <li>I/O-intensive database workloads</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Volume size</b></td>
      <td colspan="2" align="center" style="text-align: center;" tabindex="-1">1 GiB - 16 TiB </td>
      <td tabindex="-1">4 GiB - 64 TiB <sup>4</sup></td>
      <td tabindex="-1">4 GiB - 16 TiB </td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Max IOPS</b></td>
      <td tabindex="-1">16,000 (64 KiB I/O <sup>6</sup>)</td>
      <td tabindex="-1">16,000 (16 KiB I/O <sup>6</sup>)</td>
      <td tabindex="-1">256,000 <sup>5</sup> (16 KiB I/O <sup>6</sup>) </td>
      <td tabindex="-1">64,000 (16 KiB I/O <sup>6</sup>)</td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Max throughput</b></td>
      <td tabindex="-1">1,000 MiB/s</td>
      <td tabindex="-1">250 MiB/s <sup>1</sup></td>
      <td tabindex="-1">4,000 MiB/s</td>
      <td tabindex="-1">1,000 MiB/s <sup>2</sup></td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Amazon EBS Multi-attach</b></td>
      <td colspan="2" align="center" style="text-align: center;" tabindex="-1">Not supported</td>
      <td colspan="2" align="center" style="text-align: center;" tabindex="-1">Supported</td>
    </tr>
    <tr>
      <td tabindex="-1"><b>NVMe reservations</b></td>
      <td colspan="2" align="center" style="text-align: center;" tabindex="-1">Not supported</td>
      <td tabindex="-1">Supported</td>
      <td tabindex="-1">Not supported</td>
    </tr>
    <tr>
      <td tabindex="-1"><b>Boot volume</b></td>
      <td colspan="4" align="center" style="text-align: center;" tabindex="-1">Supported</td>
    </tr>
  </tbody>
</table>
