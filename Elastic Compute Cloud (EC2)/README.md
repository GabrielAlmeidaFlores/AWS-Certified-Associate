# Elastic Compute Cloud (EC2)

O Amazon Elastic Compute Cloud (Amazon EC2) oferece uma capacidade de computação escalável sob demanda na Nuvem Amazon Web Services (AWS). O uso do Amazon EC2 reduz os custos de hardware para que você possa desenvolver e implantar aplicações com mais rapidez. É possível usar o Amazon EC2 para executar quantos servidores virtuais forem necessários, configurar a segurança e as redes e gerenciar o armazenamento. Você pode adicionar capacidade (aumentar a escala verticalmente) para lidar com tarefas de computação pesada, como processos mensais ou anuais ou picos no tráfego do site. Quando o uso diminui, você pode reduzir a capacidade (reduzir a escala verticalmente) de novo.

Uma instância do EC2 é um servidor virtual na Nuvem AWS. Quando executa uma instância do EC2, o tipo de instância que você especifica determina o hardware disponível para sua instância.

## EC2 Instance Types

O Amazon EC2 fornece uma ampla seleção de tipos de instância otimizadas para de adequarem a diferentes casos de uso. Os tipos de instância incluem combinações variadas de capacidade de CPU, memória, armazenamento e redes e oferecem a flexibilidade de escolher a combinação de recursos adequada para suas aplicações. Cada tipo de instância inclui um ou mais tamanhos de instância, permitindo que você escale seus recursos de acordo com os requisitos de sua workload de destino.

Here is a table summarizing the different EC2 instance families, their associated storage types, and ideal use cases.

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

## EC2 Storage Types

Amazon EC2 provides you with flexible, cost effective, and easy-to-use data storage options for your instances. Each option has a unique combination of performance and durability. These storage options can be used independently or in combination to suit your requirements.

### Block Storage vs. File Storage vs. Object Storage

Block, object, and cloud file storage are three ways of storing data in the cloud so that users and applications can access it remotely over a network connection. Object storage stores and manages all data in an unstructured format and in units called objects. Block storage takes any data, like a file or database entry, and divides it into blocks of equal sizes. It then stores the data block on underlying physical storage in a way that’s optimized for fast access and retrieval. Cloud file storage is another data storage method that provides servers and applications access to data through shared file systems. Each type offers its own unique advantages for various use cases.

Key differences between object storage, block storage, and file storage:

| **Aspect**        | **Block Storage**                                                                           | **File Storage**                                                     | **Object Storage**                                                               |
| ----------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Definition**    | Stores data in fixed-sized blocks, accessible at the raw disk level.                        | Organizes data as files within a hierarchical directory structure.   | Stores data as objects, each containing data, metadata, and a unique identifier. |
| **Access Method** | Low-level, block-by-block access. Requires a filesystem for readability (e.g., NTFS, ext4). | File-level access with a high-level interface (e.g., NFS, SMB).      | Accessed via APIs (e.g., REST, S3 API), not a traditional filesystem.            |
| **Use Cases**     | Databases, virtual machines, OS boot drives, high-performance apps.                         | Shared file storage, backups, document storage, collaboration tools. | Cloud storage, backups, unstructured data (e.g., photos, videos, logs).          |
| **Examples**      | HDD, SSD, SAN, iSCSI, AWS EBS, Azure Disk.                                                  | NFS, CIFS/SMB, Amazon FSx, Google Drive.                             | Amazon S3, Google Cloud Storage, Azure Blob Storage, MinIO.                      |
| **Performance**   | Typically faster for random read/write operations.                                          | Optimized for file-level operations like sharing and collaboration.  | High scalability but may have higher latency for individual objects.             |
| **Bootable**      | Yes, because it allows direct, raw access for bootloaders and OS installation.              | No, as it lacks raw access for booting.                              | No, designed for data storage rather than booting.                               |
| **Scalability**   | Scales with storage volumes (block devices).                                                | Scales for user or application file access.                          | Highly scalable, designed for massive amounts of unstructured data.              |
| **Management**    | Requires partitioning and formatting for use.                                               | Easier to use; already managed by a storage system.                  | Simple, managed via metadata and APIs, no partitioning needed.                   |
