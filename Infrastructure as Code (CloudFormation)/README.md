# CloudFormation

AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that.

## CloudFormation Physical and Logical Resources

### Logical Resources

Logical Resources are the names you give to resources within your CloudFormation template. These are the identifiers you use when defining resources, like `"MyBucket"` or `"MyEC2Instance"`. They exist only within the CloudFormation template and help CloudFormation track and manage your resources during stack creation and updates.

### Physical Resources

Physical Resources are the actual real-world resources created by CloudFormation based on your template. For example, the actual S3 bucket that CloudFormation creates may be named something like `"my-stack-bucket-12345"`. This is the resource that AWS manages, and it has a unique identifier in the AWS system.
