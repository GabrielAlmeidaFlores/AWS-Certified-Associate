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

- **Inline Policies**: : An inline policy is a policy that is embedded directly within a specific IAM user, group, or role. It is tightly coupled to that principal and cannot be reused or shared across multiple principals. Inline policies are ideal when you need to apply a unique, specific set of permissions to a particular user, group, or role, and you want to ensure that the policy is deleted when the principal is removed. This can be useful in cases where precise, granular access control is needed for a single entity, and you want the policy's lifecycle to align with the lifecycle of the principal it is attached to.

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
