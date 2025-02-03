## EC2 Bootstrap

Bootstrapping in Amazon EC2 (Elastic Compute Cloud) is a powerful feature that enables automation of instance configuration during the launch process. By leveraging User Data, administrators can automate tasks such as software installation, configuration updates, and system initialization.

Key Concepts:

- **Bootstrapping and EC2 Build Automation**: Bootstrapping allows for the automation of EC2 instance setup and configuration during the launch process. It eliminates the need for manual intervention by executing predefined scripts or commands when the instance starts.

- **User Data**: User Data is a mechanism to pass configuration scripts or data to an EC2 instance at launch. It is accessed by the instance via the meta-data API at the following URL: `http://169.254.169.254/latest/user-data`

- **Execution of User Data**: User Data is executed only once during the instance's initial launch. The EC2 service itself does not interpret or execute the User Data; it is the responsibility of the instance's OS to understand and process the provided script or data.

- **Security Considerations**: User Data is not secure and should not be used to store sensitive information such as passwords, API keys, or long-term credentials. It is transmitted and stored in plaintext, making it vulnerable to exposure.

- **Size Limitations**: User Data is limited to 16 KB in size. For larger configurations, consider using external resources like Amazon S3 or a configuration management tool.

- **Modification of User Data**: User Data can be modified when the instance is in a stopped state. However, the modified User Data will only be executed during the next launch of the instance, not during a restart or reboot.

- **Monitor Execution**: Check instance logs (e.g., `/var/log/cloud-init-output.log` for Linux) to verify that User Data scripts executed successfully.

## EC2 Instance Roles

Amazon EC2 Instance Roles provide a secure and scalable way to manage permissions for applications running on EC2 instances. Instead of embedding long-term credentials (like access keys) directly into the instance, you can assign an IAM (Identity and Access Management) role to the instance. This role grants temporary security credentials to the applications running on the instance, allowing them to interact with other AWS services securely.

EC2 Instance Roles Key Concepts:

- **How It Works**: When an IAM role is attached to an EC2 instance, AWS automatically rotates and provides temporary credentials to the instance. These credentials are accessible via the Instance Metadata Service (IMDS) at: `http://169.254.169.254/latest/meta-data/iam/security-credentials/<role-name>`

- **Benefits of Using Instance Roles**

  - **Enhanced Security**: Eliminates the need to store long-term credentials on the instance.
  - **Automatic Credential Rotation**: AWS automatically rotates the temporary credentials, reducing the risk of credential exposure.
  - **Granular Permissions**: IAM roles allow you to define fine-grained permissions for specific AWS services and actions.
  - **Scalability**: Easily attach the same role to multiple instances, ensuring consistent permissions across your infrastructure.

- **Attaching an IAM Role to an EC2 Instance**: An IAM role can be attached to an EC2 instance during launch or after the instance is running. The role must have a trust policy that allows the EC2 service to assume the role.

- **Instance Metadata Service (IMDS)**: The IMDS provides a secure way for instances to retrieve metadata, including IAM role credentials. Access to IMDS can be controlled using IMDSv2, which adds an additional layer of security by requiring a token for access.

- **Limitations**
  - **Role Attachment Limit**: An EC2 instance can have only one IAM role attached at a time.
  - **Credential Lifetime**: Temporary credentials provided by the role are valid for a maximum of 6 hours.
  - **IMDS Access**: If IMDS is disabled, the instance cannot retrieve role credentials.

## SSM Parameter Store

AWS Systems Manager Parameter Store is a secure, scalable, and hierarchical storage service for configuration data and secrets. It enables developers and administrators to centrally manage application parameters, environment variables, and sensitive information across AWS services.

SSM Key Features:

- **Storage for Configuration & Secrets**: Stores configuration values, secrets, API keys, database connection strings, and other sensitive data. Reduces the need to hardcode sensitive information in applications.

- **Parameter Types**

  - **String**: Stores plaintext values (e.g., "AppConfig=Enabled").
  - **StringList**: Stores comma-separated values (e.g., "us-east-1,us-west-2").
  - **SecureString**: Encrypted values using AWS KMS, ideal for passwords and secrets.

- **Use Cases**

  - Storing API keys, database credentials, and tokens securely.
  - Centralized configuration management for applications across environments.
  - Dynamic configuration updates without modifying application code.
  - Secret management without additional services like AWS Secrets Manager.

- **Hierarchical Structure (Folder-like)**: Uses a slash-separated naming convention for better organization. Allows retrieving all parameters under a specific path (`/my-app/prod/`) using APIs or AWS CLI. Example:

- **Versioning**: Each parameter is versioned, enabling rollback and tracking of changes. Example: `/my-app/prod/db-password` may have versions 1, 2, 3.

## CloudWatch Agent

You can collect metrics from servers by installing the CloudWatch agent on the server. You can install the agent on both Amazon EC2 instances and on-premises servers. You can also install the agent on computers running Linux, Windows Server, or macOS. If you install the agent on an Amazon EC2 instance, the metrics the agent collects are in addition to the metrics enabled by default on Amazon EC2 instances.

The unified CloudWatch agent enables you to do the following:

- Collect internal system-level metrics from Amazon EC2 instances across operating systems. The metrics can include in-guest metrics, in addition to the metrics for EC2 instances. The additional metrics that can be collected are listed in Metrics collected by the CloudWatch agent.
- Collect system-level metrics from on-premises servers. These can include servers in a hybrid environment as well as servers not managed by AWS.
- Retrieve custom metrics from your applications or services using the StatsD and collectd protocols. StatsD is supported on both Linux servers and servers running Windows Server. collectd is supported only on Linux servers.
- Collect logs from Amazon EC2 instances and on-premises servers, running either Linux or Windows Server.
