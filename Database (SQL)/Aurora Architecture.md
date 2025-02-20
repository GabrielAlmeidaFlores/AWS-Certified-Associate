# Amazon Aurora Architecture

Amazon Aurora is a fully managed relational database engine developed by Amazon Web Services (AWS), designed to offer high performance, scalability, and availability. Aurora is compatible with MySQL and PostgreSQL, allowing seamless integration with existing applications that utilize these databases. Its architecture significantly differs from traditional Amazon RDS setups, primarily due to its unique cluster-based design.

## Cluster-Based Architecture

In Aurora, a database is organized into a cluster comprising a primary (writer) instance and, optionally, multiple Aurora Replicas (reader instances). This structure enables efficient handling of read and write operations, as well as enhanced availability.

- **Primary (Writer) Instance**: This instance manages all write operations and updates to the cluster's shared storage volume. Each Aurora cluster has a single primary instance responsible for processing data modifications.

- **Aurora Replicas (Reader Instances)**: These instances are dedicated to read operations, connecting to the same shared storage as the primary instance. An Aurora cluster can support up to 15 replicas, which can be distributed across multiple Availability Zones (AZs) to improve read scalability and fault tolerance. In the event of a primary instance failure, Aurora can automatically promote a replica to become the new primary instance.

## Shared Cluster Storage

Aurora utilizes a distributed and shared storage architecture, where the cluster volume spans multiple AZs within a single AWS Region. This design ensures high availability and durability by replicating data across six storage nodes—two in each of three AZs. The storage is entirely SSD-based, providing high input/output operations per second (IOPS) and low latency. Notably, Aurora's storage automatically scales as needed, up to a maximum of 128 tebibytes (TiB), eliminating the need for manual provisioning.

## Storage Billing and Management

Aurora's storage billing is based on the actual data usage, following a "high-water mark" model. This means that you are billed for the maximum storage space used during the billing period. For instance, if your cluster's storage peaked at 100 GB but later reduced to 80 GB, you would still be billed for 100 GB. To reduce storage costs associated with unused data, you would need to create a new cluster and migrate the data, as storage allocation does not automatically decrease.

## Replication and Endpoints

Aurora allows the addition or removal of replicas without requiring additional storage provisioning, thanks to its shared storage architecture. The cluster provides two primary endpoints to manage database connections:

- **Cluster Endpoint**: This endpoint directs traffic to the primary instance, handling both read and write operations.

- **Reader Endpoint**: This endpoint load-balances incoming connections across all available Aurora Replicas, facilitating read scalability.

## Backup and Restore

Aurora offers automated backups that function similarly to those in Amazon RDS. Backups are continuous and incremental, allowing for point-in-time recovery within the specified retention period, which can range from 1 to 35 days. Restoring data creates a new DB cluster, ensuring that the original cluster remains unaffected during the restoration process.

## Backtrack Feature

Aurora's Backtrack feature enables in-place rewinds of a DB cluster to a previous point in time without the need to restore data from a backup. This functionality is particularly useful for recovering from accidental data changes or deletions, as it allows for quick and efficient restoration to a known good state.

## Instance Classes and Pricing

It's important to note that Aurora does not offer a free-tier option and does not support micro instances. The service is designed for production workloads that require high availability and performance. Pricing is based on the instance class chosen, storage consumption, and I/O operations performed. For detailed pricing information, refer to the official AWS pricing page.
