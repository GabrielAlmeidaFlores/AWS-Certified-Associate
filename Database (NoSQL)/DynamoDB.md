# DynamoDB

DynamoDB is a NoSQL database service that eliminates the need for server management and infrastructure maintenance. It is a public Database-as-a-Service (DBaaS) that supports key-value and document data models. DynamoDB is highly resilient, with data replicated across multiple Availability Zones (AZs) and optionally across global regions. It offers single-digit millisecond latency for data access, thanks to its SSD-based storage. DynamoDB also provides features like backups, point-in-time recovery, and encryption at rest. It integrates with other AWS services for event-driven workflows and is billed based on read capacity units (RCUs), write capacity units (WCUs), storage, and additional features.

## Key Features

- **NoSQL Database**: DynamoDB supports both key-value and document data models, allowing for flexible schema design.

- **Fully Managed**: AWS handles all aspects of server management, including hardware provisioning, software patching, and scaling.

- **High Performance**: DynamoDB delivers single-digit millisecond latency for data access, making it suitable for high-performance applications.

- **Scalability**: DynamoDB automatically scales to accommodate changes in workload, ensuring consistent performance.

- **High Availability**: Data is replicated across multiple Availability Zones, and optionally across global regions, ensuring high availability and durability.

- **Security**: DynamoDB provides encryption at rest and integrates with AWS Identity and Access Management (IAM) for fine-grained access control.

- **Backup and Recovery**: DynamoDB supports on-demand backups and point-in-time recovery, allowing for data protection and recovery.

- **Event-Driven Integration**: DynamoDB integrates with AWS Lambda for event-driven workflows, enabling automated actions in response to data changes.

## Tables

In DynamoDB, a table is a collection of items, where each item is a collection of attributes. Tables are the primary data structure in DynamoDB, and they are designed to store and retrieve any amount of data, serving any level of request traffic.

### Items and Attributes

An item in DynamoDB is a single data record in a table. Each item is uniquely identified by a primary key, which can be either a single attribute (partition key) or a combination of two attributes (partition key and sort key). Items in DynamoDB can have different attributes, and the database does not enforce a rigid schema. This flexibility allows for a wide range of data models and use cases.

- **Primary Key**: The primary key uniquely identifies each item in a table. It can be a single attribute (partition key) or a combination of two attributes (partition key and sort key).

- **Attributes**: Attributes are the data elements that make up an item. Each attribute has a name and a value, and the value can be a scalar, set, or document type.

### Item Size Limit

Each item in DynamoDB has a maximum size limit of 400 KB. This limit includes the attribute names, values, and any overhead associated with the item. It is important to design your data model with this limit in mind to ensure efficient storage and retrieval.

## Capacity

DynamoDB allows you to control the performance of your tables by setting the read and write capacity units. Capacity units determine the throughput of your table, and you can choose between provisioned capacity and on-demand capacity modes.

### Read Capacity Units (RCUs)

Read capacity units (RCUs) determine the number of read operations that can be performed on a table per second. One RCU represents one strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size.

- **Strongly Consistent Reads**: Strongly consistent reads return the most up-to-date data, reflecting all successful writes that occurred before the read operation. One RCU provides one strongly consistent read per second for items up to 4 KB in size.

- **Eventually Consistent Reads**: Eventually consistent reads may return stale data, as they do not guarantee that the most recent write will be reflected. One RCU provides two eventually consistent reads per second for items up to 4 KB in size.

### Write Capacity Units (WCUs)

Write capacity units (WCUs) determine the number of write operations that can be performed on a table per second. One WCU represents one write per second for items up to 1 KB in size.

- **Write Operations**: Each write operation consumes one WCU, regardless of whether it is a new item, an update to an existing item, or a deletion. If the item size exceeds 1 KB, additional WCUs are consumed.

### Capacity Modes

DynamoDB offers two capacity modes: provisioned and on-demand. The choice of capacity mode depends on your application's workload and performance requirements.

- **Provisioned Capacity**: In provisioned capacity mode, you specify the number of RCUs and WCUs for your table. This mode is suitable for applications with predictable workloads, where you can estimate the required throughput in advance.

- **On-Demand Capacity**: In on-demand capacity mode, DynamoDB automatically scales the throughput based on the actual workload. This mode is suitable for applications with unpredictable or variable workloads, where it is difficult to estimate the required throughput in advance.

## Backups

DynamoDB provides two types of backups: on-demand backups and point-in-time recovery. These features ensure that your data is protected and can be recovered in the event of accidental deletion or corruption.

### On-Demand Backups

On-demand backups allow you to create a full copy of your table at any point in time. These backups are retained until you explicitly delete them, and they can be used to restore your table to a specific state.

- **Full Copy**: On-demand backups create a full copy of your table, including all items, attributes, and indexes.

- **Cross-Region Restore**: You can restore a table from an on-demand backup in another AWS region, providing additional flexibility for disaster recovery.

- **Encryption Settings**: You can adjust the encryption settings when restoring a table from a backup, ensuring that your data remains secure.

### Point-in-Time Recovery

Point-in-time recovery (PITR) provides continuous backups of your table, allowing you to restore your table to any point in time within a 35-day recovery window.

- **Continuous Backups**: PITR continuously records changes to your table, enabling you to replay these changes to restore your table to a specific point in time.

- **Recovery Window**: PITR provides a 35-day recovery window, during which you can restore your table to any point in time.

- **Not Enabled by Default**: PITR is not enabled by default, and you must explicitly enable it for each table.

## Retrieving Data from a Table

DynamoDB provides two primary methods for retrieving data from a table: query and scan. Each method has its own use cases and performance characteristics.

### Query

The query operation is the most efficient way to retrieve data from DynamoDB. It allows you to retrieve items based on the primary key or a combination of the primary key and sort key.

- **Primary Key**: The query operation requires a single primary key value, and optionally, a sort key value or range.

- **Capacity Consumption**: The capacity consumed by a query operation is based on the size of all items returned. Additional filtering may discard data, but the capacity is still consumed based on the size of the scanned items.

- **Flexibility**: Query operations are limited to retrieving data based on the primary key and sort key. They cannot be used to retrieve data based on other attributes.

### Scan

The scan operation is the most flexible way to retrieve data from DynamoDB, but it is also the least efficient. It allows you to retrieve all items in a table, regardless of the primary key.

- **Full Table Scan**: The scan operation moves through the entire table, consuming capacity for every item scanned.

- **Filtering**: You can apply filters to the scan operation to retrieve specific items, but the capacity is still consumed for every item scanned.

- **Performance Impact**: Scan operations can have a significant impact on performance, especially for large tables. They should be used sparingly and only when necessary.

## Consistency Model

DynamoDB uses a distributed architecture to ensure high availability and durability. Data is replicated across multiple storage nodes, and each node is located in a different Availability Zone. One of these nodes is elected as the leader node, which is responsible for coordinating write operations.

### Eventually Consistent Reads

Eventually consistent reads provide a lower-cost option for retrieving data, but they may return stale data if the replication process has not completed.

- **Replication Delay**: Eventually consistent reads may check a node that has not yet received the latest updates, resulting in stale data.

- **Cost**: Eventually consistent reads consume half the capacity of strongly consistent reads, making them a cost-effective option for applications that can tolerate stale data.

### Strongly Consistent Reads

Strongly consistent reads provide the most up-to-date data by connecting to the leader node.

- **Leader Node**: Strongly consistent reads always connect to the leader node, ensuring that the most recent data is returned.

- **Cost**: Strongly consistent reads consume more capacity than eventually consistent reads, but they provide the most accurate data.

## DynamoDB Indexes

Indexes in DynamoDB provide alternative views of your table data, allowing you to query data using different attributes. DynamoDB supports two types of indexes: local secondary indexes (LSIs) and global secondary indexes (GSIs).

### Local Secondary Indexes (LSIs)

Local secondary indexes (LSIs) provide an alternative view of your table data using the same partition key but a different sort key.

- **Creation**: LSIs must be created when the table is created, and they cannot be added later.

- **Limit**: You can create up to five LSIs per table.

- **Capacity**: LSIs share the read and write capacity of the base table.

- **Attributes**: LSIs can project all attributes, only the key attributes, or a subset of attributes.

### Global Secondary Indexes (GSIs)

Global secondary indexes (GSIs) provide an alternative view of your table data using a different partition key and sort key.

- **Creation**: GSIs can be created at any time, and they can be added to an existing table.

- **Limit**: You can create up to 20 GSIs per table by default.

- **Capacity**: GSIs have their own read and write capacity, separate from the base table.

- **Attributes**: GSIs can project all attributes, only the key attributes, or a subset of attributes.

- **Consistency**: GSIs always use eventually consistent reads.

### Differences Between LSIs and GSIs

- **Use Cases**: Use GSIs as the default option for alternative access patterns. Use LSIs only when strong consistency is required.

- **Flexibility**: GSIs provide more flexibility, as they can be created at any time and have their own capacity settings.

## Streams and Triggers

DynamoDB streams provide a time-ordered list of item changes in a table, enabling event-driven workflows.

### Streams

DynamoDB streams record changes to items in a table, including inserts, updates, and deletes. These changes are stored in a rolling 24-hour window, allowing you to react to changes in real-time.

- **View Types**: You can configure the stream to record different types of changes, including keys only, new images, old images, or both new and old images.

- **Integration**: Streams can be integrated with AWS Lambda to trigger actions in response to changes in the table.

### Triggers

Triggers are actions that are automatically executed in response to changes in a DynamoDB table. These changes are captured by the stream and passed to a Lambda function, which performs the desired action.

- **Event-Driven Workflows**: Triggers enable event-driven workflows, where changes in the table automatically trigger actions in other AWS services.

- **Lambda Integration**: AWS Lambda is the primary service used to implement triggers, allowing you to execute custom code in response to changes in the table.

## DynamoDB Accelerator (DAX)

DynamoDB Accelerator (DAX) is an in-memory cache that provides microsecond latency for read-heavy workloads.

### Architecture

DAX is deployed within a Virtual Private Cloud (VPC) and uses clusters to provide high availability and scalability.

- **Clusters**: DAX clusters consist of a primary node and replica nodes. The primary node handles write operations, while the replica nodes handle read operations.

- **Endpoint**: Applications access DAX through an endpoint, which routes requests to the appropriate node.

- **Cache**: DAX caches the results of read operations, reducing the load on the underlying DynamoDB table.

### Cache Invalidation

DAX uses a write-through cache model, where write operations are immediately written to both the cache and the underlying DynamoDB table. This ensures that the cache is always up-to-date with the latest data.

- **Write-Through**: Write operations are written to both the cache and the DynamoDB table, ensuring consistency.

- **Cache Misses**: If a cache miss occurs, DAX retrieves the data from the DynamoDB table and caches it for future requests.

### Performance

DAX provides significant performance improvements for read-heavy workloads, reducing latency and costs.

- **Latency**: Cache hits are returned in microseconds, while cache misses are returned in milliseconds.

- **Scalability**: DAX can scale up by increasing the size of the nodes or scale out by adding more nodes to the cluster.

## Global Tables

Global tables provide multi-master, cross-region replication for DynamoDB tables, enabling low-latency access to data from anywhere in the world.

### Multi-Master Replication

Global tables use multi-master replication, allowing you to read and write data in any region.

- **Replica Tables**: Tables are created in multiple regions and added to the same global table, becoming replica tables.

- **Conflict Resolution**: In the event of conflicting writes, the last writer wins, ensuring that the most recent update is applied.

- **Replication**: Replication between regions is asynchronous, with sub-second latency.

### Consistency

Global tables provide strong consistency within the same region as the write operation, but eventual consistency across regions.

- **Strong Consistency** Strongly consistent reads are only available in the same region as the write operation.

- **Eventual Consistency** Reads in other regions may return stale data until the replication process completes.

## DynamoDB TTL (Time to Live)

DynamoDB TTL allows you to automatically delete items from a table after a specified period of time.

### TTL Attribute

TTL is enabled by selecting a specific attribute in the table that contains the expiration time.

- **Timestamp**: The TTL attribute must contain a timestamp in Unix epoch time format, representing the expiration time for the item.

- **Background Process**: A background process periodically checks the TTL attribute and deletes expired items.

### Performance Impact

TTL deletions are performed by background processes and do not impact the performance of the table.

- **Background Deletion**: Expired items are removed from the table and indexes by background processes, ensuring that the table performance is not affected.
- **Streams**: If streams are enabled, a delete operation is added to the stream when an item is deleted by TTL.
