## 01

A gaming website gives users the ability to trade game items with each other on the platform. The platform requires both users' records to be updated and persisted in one transaction. If any update fails, the transaction must roll back.
Which AWS solution can provide the transactional capability that is required for this feature?

A. Amazon DynamoDB with operations made with the Consistent Read parameter set to true
B. Amazon ElastiCache for Memcached with operations made within a transaction block
C. Amazon DynamoDB with reads and writes made by using Transact\* operations
D. Amazon Aurora MySQL with operations made within a transaction block
E. Amazon Athena with operations made within a transaction block

<details>
<summary>**Answer**</summary>
<br>

The correct answer is:

**C. Amazon DynamoDB with reads and writes made by using Transact\* operations**

### Explanation

- **Amazon DynamoDB** supports **TransactWriteItems** and **TransactGetItems** operations, which allow you to perform multiple reads and writes as a single, atomic transaction. This ensures that either all the operations in the transaction succeed, or none of them do, providing the transactional capability required for the gaming platform's item trading feature.
- **Option A** is incorrect because the "Consistent Read" parameter ensures strong consistency for individual read operations but does not provide transactional capabilities for multiple operations.
- **Option B** is incorrect because Amazon ElastiCache for Memcached does not support transactional operations.
- **Option D** is incorrect because while Amazon Aurora MySQL supports transactions, it is not the best fit for a gaming platform that likely requires high scalability and low latency, which DynamoDB is better suited for.
- **Option E** is incorrect because Amazon Athena is a query service for analyzing data in S3 and does not support transactional operations.

</details>
