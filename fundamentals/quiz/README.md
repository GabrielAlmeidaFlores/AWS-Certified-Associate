## What Permissions options does an AMI have?

- a. Public Access, Owner only, Specific AWS Accounts
- b. Public Access, Owner only, Specific IAM users
- c. Public Access, Owner only, Specific Regions
- d. Public Access, Specific AWS Accounts, Specific IAM users

<details>
  <summary>Answer</summary>

a. Public Access, Owner only, Specific AWS Accounts

</details>

<details>
  <summary>Explanation</summary>

Public Access, Owner only, Specific AWS Accounts

The correct option for an AMI's permissions is Public Access, Owner only, Specific AWS Accounts because AMIs are by default private, accessible only to the owner (the AWS account that created it). The owner can choose to share the AMI with specific AWS accounts, allowing them to launch or copy the image, or make it publicly available to all AWS users.

This setup ensures that the AMI is secured by default, but can still be shared with trusted users or organizations. It provides a flexible way for owners to control access based on the needs of their environment, such as sharing the AMI with specific AWS accounts while keeping it private from others.

</details>
