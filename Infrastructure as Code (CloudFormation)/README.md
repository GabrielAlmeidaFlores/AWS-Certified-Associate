# CloudFormation

AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that.

## CloudFormation Template

In AWS CloudFormation, a CloudFormation template is a JSON or YAML file that defines the resources you want to create and configure in your AWS environment. It acts as a blueprint for CloudFormation to provision and manage your infrastructure. It describes things like EC2 instances, S3 buckets, IAM roles, and other AWS resources, as well as their properties and dependencies.

There are multiple primary sections that make up a CloudFormation template: `Resources`, `Outputs`, `Conditions`, `Mappings`, and `Parameters`

### Resources

The Resources section is the core part of a CloudFormation template. It defines the AWS resources that CloudFormation will create, such as EC2 instances, S3 buckets, Lambda functions, security groups, and more.

Each resource is defined by its logical ID and resource type. The logical ID is a unique name that references the resource within the template, and the resource type is the specific AWS service being created (e.g., `AWS::EC2::Instance`, `AWS::S3::Bucket`).

Example:

```YAML
Resources:
  MyBucket:
    Type: "AWS::S3::Bucket"
    Properties:
      BucketName: "my-unique-bucket-name"
```

In this example, a new S3 bucket (`AWS::S3::Bucket`) is defined with the logical ID MyBucket, and its name is specified in the properties.

### Output

The Outputs section specifies the values that CloudFormation returns when a stack is successfully created or updated. Outputs are useful for displaying values you may want to reference later, such as resource identifiers (e.g., S3 bucket name, EC2 instance ID).

You can also use outputs to provide references to other stacks in a cross-stack scenario.

Example:

```YAML
Outputs:
  BucketName:
    Description: "Name of the S3 bucket"
    Value: !Ref MyBucket
```

In this example, the output `BucketName` returns the name of the `MyBucket` S3 bucket that was created.

### Parameters

The Parameters section allows you to define inputs to your CloudFormation stack. These are values you can specify when you create or update the stack, making the template dynamic and flexible. Parameters allow you to reuse the same template for different environments or configurations.

You can specify things like instance sizes, AMI IDs, or key names as parameters.

Example:

```YAML
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium
    Description: "EC2 instance type"
```

In this example, the `InstanceType` parameter allows the user to specify an EC2 instance type, with a default of `t2.micro`.

### Conditions

The Conditions section defines conditions that control when certain resources are created or modified. Conditions can be used to create resources only if specific criteria are met, allowing for more flexible stack deployments based on parameters or other values.

For example, you might use a condition to create a resource only if the stack is being deployed in a specific region or only if a parameter value is set to a certain value.

Example:

```YAML
Conditions:
  CreateProdResources: !Equals [!Ref Environment, "prod"]

Resources:
  ProdBucket:
    Type: "AWS::S3::Bucket"
    Condition: CreateProdResources
    Properties:
      BucketName: "prod-bucket"
```

In this example, the `ProdBucket` resource will only be created if the Environment parameter is set to `"prod"`.
