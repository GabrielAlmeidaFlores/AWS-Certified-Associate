# Aurora Serverless

Amazon Aurora Serverless is an on-demand, auto-scaling configuration for Amazon Aurora, designed to automatically adjust database capacity based on application needs. This eliminates the need for manual provisioning and management of database instances, offering a cost-effective solution for applications with variable or unpredictable workloads. Aurora Serverless is available in two versions: Aurora Serverless v1 and the more advanced Aurora Serverless v2.

## Aurora Capacity Units (ACUs) and Scaling

Aurora Serverless utilizes Aurora Capacity Units (ACUs) to measure database capacity. Each ACU comprises approximately 2 gibibytes (GiB) of memory, along with corresponding CPU and networking resources. Users define a minimum and maximum ACU range for their Aurora Serverless clusters, allowing the service to scale seamlessly within these parameters based on real-time application demands. Notably, Aurora Serverless v2 offers the capability to scale down to 0 ACUs, enabling the database to pause during periods of inactivity and resume when needed, thereby optimizing cost efficiency.

## Consumption-Based Billing

Billing for Aurora Serverless is consumption-based, calculated on a per-second basis. Charges are incurred only for the database capacity actively used, ensuring cost-effectiveness, especially for workloads that do not require constant database activity.

## High Availability and Resilience

Aurora Serverless maintains the same high availability and resilience as standard Amazon Aurora deployments. It replicates data across multiple Availability Zones (AZs), with six copies of data distributed to protect against data loss and ensure fault tolerance.

## Ideal Use Cases for Aurora Serverless

Infrequently Used Applications: Applications that are accessed sporadically, such as low-traffic blogs or internal tools, benefit from Aurora Serverless by incurring costs only during active periods.

- **New Applications with Uncertain Demand**: Startups or new projects can leverage Aurora Serverless to accommodate unknown usage patterns without the risk of over-provisioning resources.

- **Variable or Unpredictable Workloads**: Applications experiencing fluctuating traffic, such as seasonal e-commerce platforms or news sites with occasional surges, can automatically scale resources up or down as needed.

- **Development and Testing Environments**: Developers can utilize Aurora Serverless for testing purposes, allowing databases to scale down to zero during inactivity, thus reducing costs.

- **Multi-Tenant Applications**: SaaS providers hosting multiple client databases can manage varying workloads efficiently, as Aurora Serverless adjusts capacity for each tenant based on demand.

## Architecture of Aurora Serverless

The architecture of Aurora Serverless is designed to provide seamless scalability and high availability:

- **Aurora Serverless Cluster**: This cluster comprises a collection of compute resources that automatically adjust capacity based on the defined ACU range and current workload.

- **Aurora Capacity Units (ACUs)**: ACUs are stateless and do not possess local storage. They are dynamically allocated from a shared pool as demand increases and are removed when no longer needed, ensuring efficient resource utilization.

- **Proxy Fleet**: A fleet of proxy instances manages client connections to the database. Clients connect to the proxy fleet, which in turn routes requests to the appropriate ACUs. This layer abstracts the underlying compute resources, allowing for transparent scaling operations without impacting client applications.

## Transition from Aurora Serverless v1 to v2

It's important to note that AWS has announced the end-of-life date for Aurora Serverless v1 as March 31, 2025. Users are strongly encouraged to upgrade to Aurora Serverless v2 before this date to benefit from improved scaling capabilities, enhanced feature support, and continued service. Aurora Serverless v2 offers faster and more granular scaling, better integration with other AWS services, and support for a broader range of features compared to its predecessor.
