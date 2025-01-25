## IAM Policies

AWS IAM Policies define permissions for actions that can be performed on AWS resources. They are used to grant access to users, groups, or roles within AWS. IAM (Identity and Access Management) is a service that helps manage access to AWS resources in a secure manner.

In AWS IAM policies, Deny takes precedence over Allow, meaning that if a policy explicitly denies access to a resource, it will override any permissions that allow access to the same resource. Additionally, if no explicit Allow permission is granted for a specific action or resource, the default behavior is Implicit Deny. This means that by default, any action that is not explicitly allowed will be denied. This "implicit deny" ensures that any resource not explicitly granted permissions is inaccessible, providing a security safeguard.

The evaluation of permissions in IAM follows a priority order:

1. **Explicit Deny**: If a policy explicitly denies an action or resource, the request will be blocked, regardless of any Allow permissions elsewhere.

2. **Explicit Allow**: If a policy explicitly allows an action or resource, access is granted, but only if there is no conflicting Deny.

3. **Implicit Deny**: If there is no explicit Allow or Deny for an action, the default behavior is an implicit Deny, meaning access is automatically denied.

For example, consider the following IAM policy that grants full access to all S3 resources but explicitly denies access to the sensitive-data bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FullAccess",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    },
    {
      "Sid": "FullAccess",
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::sensitive-data",
        "arn:aws:s3:::sensitive-data/*"
      ]
    }
  ]
}
```

In this case, the user is granted full access to all S3 resources via the Allow statement. However, the Deny statement specifically blocks access to the sensitive-data bucket and its contents, ensuring that the user cannot interact with those particular resources. The Deny statement overrides any Allow permissions for the sensitive-data bucket, showing how explicit Deny takes precedence over both explicit Allow and implicit Deny, ensuring sensitive data remains secure.

### Two Types of IAM Policies

- **Inline Policies**: An inline policy is a policy that is embedded directly within a specific IAM user, group, or role. It is tightly coupled to that principal and cannot be reused or shared across multiple principals. Inline policies are ideal when you need to apply a unique, specific set of permissions to a particular user, group, or role, and you want to ensure that the policy is deleted when the principal is removed. This can be useful in cases where precise, granular access control is needed for a single entity, and you want the policy's lifecycle to align with the lifecycle of the principal it is attached to.

- **Managed Policies**: A managed policy is a standalone policy that can be created, managed, and attached to multiple users, groups, or roles across an AWS account. Managed policies are reusable, making them ideal for scenarios where you want to maintain consistent permission sets across multiple IAM entities. AWS provides pre-built managed policies for common use cases, or you can create your own custom managed policies to meet specific organizational needs. Managed policies are best suited for larger environments where access management needs to be scalable and easily maintainable.

## IAM Users

An IAM user is an identity in AWS that is used for long-term access to AWS resources. This could be for humans, applications, or service accounts that need non-temporary credentials. IAM users are typically assigned permissions to perform specific actions on AWS resources.

Here are some key points to remember:

- IAM users represent entities requiring long-term access (humans, apps, or services).
- There is a limit of 5,000 IAM users per AWS account.
- An IAM user can be a member of up to 10 groups.
- Users are assigned permissions directly or through group memberships.

## Amazon Resource Name (ARN)

An ARN is a unique identifier for AWS resources within an account. It is used to reference specific resources in IAM policies, AWS CLI commands, and other AWS services. ARNs are essential for defining which resources a policy applies to and for making resource-specific actions.

Here are some key points to remember:

- ARNs uniquely identify AWS resources within an account.
- The format includes the service name, region, account ID, and resource details.
- ARNs are used to specify resources in IAM policies, defining access control
- Examples of ARNs: S3 Bucket ARN: `arn:aws:s3:::my-bucket`, EC2 Instance ARN: `arn:aws:ec2:us-west-2:123456789012:instance/i-1234567890abcdef0`

## IAM Groups

IAM Groups is a feature in AWS that helps organize and manage IAM users more efficiently. A group acts as a container for users who share similar permission requirements, simplifying the assignment of permissions. While IAM groups do not provide direct login access, they allow you to apply policies to a collection of users at once, instead of managing permissions individually for each user.

Here are some key points to remember:

- IAM groups are used to group IAM users with similar access needs, allowing for centralized permission management.
- Groups themselves cannot log in or authenticate; they are only a means of organizing users for permission management.
- Groups streamline the process of assigning permissions by allowing policies to be applied to all members of the group, reducing the complexity of individual user permission management.
- Groups can have inline policies (specific to the group) or managed policies (reusable across multiple entities) attached to them.
- Groups cannot contain other groups, ensuring that each group only holds IAM users, not other groups.
- An AWS account can have up to 300 IAM groups, which sets a limit on how many distinct access groups you can have.
- IAM groups are not considered IAM entities that can be assigned permissions or referenced in policies as principals (e.g., you cannot grant permissions to a group directly in a policy). They serve only as a way to manage users.

## IAM Roles

IAM Roles are a way to delegate permissions to entities within AWS without using long-term credentials. Unlike IAM users, roles are not directly associated with a single user or group but are temporarily assumed by trusted entities such as users, applications, or services. When an entity assumes a role, it temporarily gains the permissions defined by that role, enabling it to perform specific actions in AWS.

For example, if an EC2 instance needs to access an S3 bucket, it can assume a role that has the necessary permissions to access the bucket. By using sts:AssumeRole, the EC2 instance avoids needing long-term credentials stored in the instance itself.

### Security Token Service (STS)

The Security Token Service (STS) is an AWS service that provides temporary security credentials for users or services to access AWS resources. These credentials consist of an access key, a secret access key, and a session token. STS is a critical component for enabling secure and temporary access to AWS resources without the need for long-term credentials.

Key Features of STS:

- Allows users, applications, or services to assume roles and gain specific permissions defined in the role.
- Credentials are short-lived and expire after a specified duration, enhancing security.
- Enables federated access for users from external identity providers to access AWS resources.
- Facilitates access to resources in another AWS account by assuming roles in that account.

### Service-linked Roles

Service-linked roles are special IAM roles that are directly associated with a specific AWS service. These roles provide the service with the permissions it needs to manage resources on your behalf. Service-linked roles are predefined by the AWS service, and the trust and permission policies are automatically managed by AWS.

Key Features of Service-linked Roles:

- The service creates and manages the role, or you can create it during setup.
- You cannot delete a service-linked role until it is no longer required by the service.

### Trust Policy vs. Permission Policy

#### Trust Policy

A trust policy defines who or what can assume a role. It specifies the trusted entities (principals) that are allowed to use the role. These principals can include AWS accounts, services, or federated users.

Example of a Trust Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

In this example, the role can be assumed by the EC2 service (ec2.amazonaws.com).

#### Permission Policy

A permission policy defines what actions an entity can perform once it has assumed the role. It specifies the resources the role can access and the actions it can perform on those resources.

Example of a Permission Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::example-bucket"
    }
  ]
}
```

In this example, the role can list the contents of the specified S3 bucket.

## AWS Organizations

AWS Organizations is a service that enables centralized management of multiple AWS accounts. It provides features like consolidated billing, hierarchical account organization, and the ability to apply policies across accounts for governance and compliance.

Key Features of AWS Organizations:

- **Management Account**: AWS Organizations has a single management account (previously known as the master account). The management account can invite other AWS accounts to join the organization as member accounts. It has elevated privileges to manage the organization and apply policies across accounts.

- **Member Accounts**: Member accounts are AWS accounts that join the organization. These accounts can be managed centrally by the management account but retain the ability to define their own IAM policies within the boundaries set by Service Control Policies (SCPs).

- **Consolidated Billing**: AWS Organizations enables consolidated billing, combining charges from multiple accounts into a single bill. This simplifies payment management and allows for volume discounts based on aggregated usage.

- **Organizational Units (OUs)**: Organizational Units (OUs) are logical groupings of AWS accounts within an organization. OUs allow hierarchical organization and policy application at different levels.

### Service Control Policies (SCP)

Service Control Policies (SCPs) are permission boundaries that define the maximum permissions available to accounts within an organization. SCPs apply to all users, roles, and entities within an account, including the root user. However, SCPs do not grant permissions; they only restrict them.

SCPs are not enabled by default when you create an AWS Organization. If SCPs are enabled, each account in the organization receives a default policy called FullAWSAccess. The FullAWSAccess SCP allows all actions and is defined as:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

Key Characteristics of SCPs:

- SCPs do not apply to the management account.
- SCPs limit the actions that IAM policies can grant within an account.
- SCPs applied at a parent OU are automatically inherited by child OUs and accounts.
- Actions granted by IAM policies are still subject to SCP restrictions.
- Use the AWS Organizations policy simulator to evaluate the impact of SCPs before implementation.
