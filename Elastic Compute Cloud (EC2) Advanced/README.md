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
