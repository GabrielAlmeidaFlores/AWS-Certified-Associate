# CloudFormation

AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that.

## CloudFormation Template

In AWS CloudFormation, a CloudFormation template is a JSON or YAML file that defines the resources you want to create and configure in your AWS environment. It acts as a blueprint for CloudFormation to provision and manage your infrastructure. It describes things like EC2 instances, S3 buckets, IAM roles, and other AWS resources, as well as their properties and dependencies.

### Template Sections

A CloudFormation template is composed of several main sections that work together to automate resource provisioning and configuration. These sections—Parameters, Conditions, Mappings, Resources, and Outputs—enable users to customize, control, and manage their cloud environment in a flexible and dynamic manner.

#### Resources

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

#### Output

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

#### Parameters

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

#### Conditions

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

### Template Functions

#### `!Ref` (Reference) Function

The `!Ref` function is one of the most commonly used functions in CloudFormation templates. It returns the value of a specified resource or parameter. If used with a resource,`!Ref` typically returns the physical ID of that resource, which could be the name of an AWS resource such as an S3 bucket, the ARN of an IAM role, or the ID of an EC2 instance. When used with a parameter, it returns the value of that parameter as provided in the template. This function is critical in connecting resources and ensuring that references between them are dynamically updated during stack creation.

Example:

```YAML
Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'

Outputs:
  BucketName:
    Value: !Ref MyBucket
```

In the above example, the `!Ref MyBucket` will resolve to the name of the S3 bucket that is created by the `MyBucket` resource. When the stack is deployed, CloudFormation will replace `!Ref MyBucket` with the actual bucket name. This function helps in establishing connections between resources in the same stack or even between stacks if outputs are shared.

#### `!GetAtt` (Get Attribute) Function

The `!GetAtt` function retrieves the value of an attribute from a resource that is defined in the template. This function is typically used when you need to access specific properties of a resource after it has been created. Unlike `!Ref`, which returns a single identifier (e.g., a name or ID), `!GetAtt` allows you to access more specific resource attributes, such as an S3 bucket's ARN or an EC2 instance's private IP address.

Example:

```YAML
Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'

Outputs:
  BucketArn:
    Value: !GetAtt MyBucket.Arn
```

In this example, `!GetAtt MyBucket.Arn` fetches the ARN of the S3 bucket created by the `MyBucket` resource. This is useful when you need to use that ARN in another resource or for output purposes. The function allows fine-grained control over resource attributes in a CloudFormation stack.

#### `!Sub` (Substitution) Function

The `!Sub function` is used to perform string substitution. It replaces variables in a string with their corresponding values, allowing dynamic string generation based on resource properties or parameters. This is particularly useful for constructing resource names, URIs, or ARNs where part of the string is dynamic and based on other resources or parameters in the template.

Example:

```YAML
Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Sub '${AWS::Region}-mybucket-${AWS::AccountId}'
```

In this example, the `!Sub` function dynamically creates a bucket name based on the AWS region and account ID. When the stack is deployed, CloudFormation will replace `${AWS::Region}` and `${AWS::AccountId}` with the current region and account ID, respectively. This allows for flexible and unique resource naming based on the environment where the stack is deployed.

#### `!Join` (Join) Function

The `!Join` function is used to concatenate multiple strings together into a single string. This is useful when constructing resource properties or ARNs that require multiple strings to be joined, such as appending prefixes or suffixes to a name. `!Join` takes two arguments: the delimiter (e.g., a space, comma, or slash) and a list of strings to join.

Example:

```YAML
Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Join ['-', ['mybucket', !Ref AWS::Region]]
```

In this example, `!Join` is used to combine the prefix `mybucket` and the region value dynamically, creating a bucket name like `mybucket-us-east-1`. This function allows you to generate more complex resource names or other properties by joining multiple string values together.

#### `!FindInMap` (Find) Function

The `!FindInMap` function is used to retrieve values from a `Mappings` section in a CloudFormation template. This is useful when defining static configurations, such as AMI IDs per region, environment-specific settings, or other lookup tables. `!FindInMap` takes three arguments: the map name, the top-level key, and the second-level key.

Example:

```YAML
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-12345678
    us-west-2:
      AMI: ami-87654321

Resources:
  MyInstance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref AWS::Region, AMI]
      InstanceType: t2.micro
```

In this example, `!FindInMap` retrieves the AMI ID based on the current AWS region. If the stack is deployed in `us-east-1`, the instance will use `ami-12345678`, whereas in `us-west-2`, it will use `ami-87654321`. This function is helpful for managing environment-specific configurations within a single CloudFormation template.

#### `!Base64` (Base64) Function

The `!Base64` function in CloudFormation is used to encode a string into Base64 format. This is commonly used when passing UserData to an EC2 instance to run commands during the instance’s bootstrap process. UserData is typically used for instance initialization tasks, such as installing software, configuring the system, or executing scripts at launch time. The `!Base64` function allows you to pass these scripts in a format that EC2 can interpret and execute.

The `!Base64` function takes a single argument: the string (or UserData script) to be encoded in Base64 format.

Example:

```YAML
Resources:
  MyInstance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0cb91c7de36eed2cb
      UserData: !Base64 |
        #!/bin/bash
        yum update -y
        yum install -y httpd
        systemctl start httpd
        systemctl enable httpd
      Tags:
        - Key: Name
          Value: "MyEC2Instance"
```

In this example, the `!Base64` function is used to encode a simple shell script into Base64. This script installs and starts the Apache HTTP server (`httpd`) on the EC2 instance during boot. The `|` symbol in YAML preserves the multi-line formatting of the script.

When the instance starts, AWS will decode the Base64 string and execute the commands in the `UserData` section. This is essential for configuring the instance to run specific applications or services right after launch.

By using `!Base64`, you can easily provide and execute custom initialization scripts on EC2 instances without manually encoding them yourself, streamlining the process of automating server configuration.

#### `!Equals` (Comparison) Function

The `!Equals` function is used to compare two values and return `true` if they are equal, or `false` if they are not. This is useful for conditions where you need to check if two values, such as parameters or resource properties, match exactly.

Example:

```YAML
Conditions:
  IsProdEnvironment: !Equals [!Ref Environment, 'prod']

Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Condition: IsProdEnvironment
```

In this example, the `!Equals` function compares the Environment parameter with the string `'prod'`. If they are equal, the condition `IsProdEnvironment` is evaluated as `true`, and the `MyBucket` resource is created.

#### `!Not` (Logical) Function

The `!Not` function is used to negate a condition or expression. It returns `true` if the given condition is `false` and returns `false` if the condition is `true`. This is useful for cases where you need the opposite result of a condition.

Example:

```YAML
Conditions:
  IsNotProdEnvironment: !Not [!Equals [!Ref Environment, 'prod']]

Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Condition: IsNotProdEnvironment
```

In this example, the `!Not` function negates the result of the `!Equals` function. If the Environment is not `'prod'`, the condition `IsNotProdEnvironment` becomes true, and the `MyBucket` resource is created.

#### `!And` (Logical) Function

The `!And` function is used to combine multiple conditions and returns `true` only if all the conditions evaluate to `true`. If any condition evaluates to `false`, the result will be `false`. This is useful when you need to check for multiple conditions simultaneously.

Example:

```YAML
Conditions:
  IsProdAndHighTraffic: !And
    - !Equals [!Ref Environment, 'prod']
    - !GreaterThan [!Ref TrafficLevel, 1000]

Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Condition: IsProdAndHighTraffic
```

In this example, the `!And` function checks two conditions: whether the `Environment` is `'prod'` and whether the `TrafficLevel` is greater than `1000`. If both conditions are `true`, the `MyBucket` resource is created.

#### `!Or` (Logical) Function

The `!Or` function is used to combine multiple conditions and returns `true` if at least one of the conditions evaluates to `true`. If all conditions evaluate to `false`, the result will be `false`. This is useful when you want to allow for multiple valid scenarios.

Example:

```YAML
Conditions:
  IsProdOrDevEnvironment: !Or
    - !Equals [!Ref Environment, 'prod']
    - !Equals [!Ref Environment, 'dev']

Resources:
  MyBucket:
    Type: 'AWS::S3::Bucket'
    Condition: IsProdOrDevEnvironment
```

In this example, the `!Or` function checks whether the `Environment` is either `'prod'` or `'dev'`. If either condition is `true`, the `MyBucket` resource will be created.

### Template Build

#### DependsOn attribute

With the `DependsOn` attribute you can specify that the creation of a specific resource follows another. When you add a `DependsOn` attribute to a resource, that resource is created only after the creation of the resource specified in the `DependsOn` attribute.

!> [!IMPORTANT]

> Dependent stacks also have implicit dependencies in the form of target properties `!Ref`, `!GetAtt`, and !Sub. For example, if the properties of resource A use a `!Ref` to resource B, the following rules apply:
>
> - Resource B is created before resource A.
> - Resource A is deleted before resource B.
> - Resource B is updated before resource A.

The following template contains an `AWS::EC2::Instance` resource with a `DependsOn` attribute that specifies `myDB`, an `AWS::RDS::DBInstance`. When `CloudFormation` creates this stack, it first creates `myDB`, then creates `Ec2Instance`.

```YAML
AWSTemplateFormatVersion: '2010-09-09'
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0ff8a91507f77f867
    us-west-1:
      AMI: ami-0bdb828fd58c52235
    eu-west-1:
      AMI: ami-047bb4163c506cd98
    ap-northeast-1:
      AMI: ami-06cd52961ce9f0d85
    ap-southeast-1:
      AMI: ami-08569b978cc4dfa10
Resources:
  Ec2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId:
        Fn::FindInMap:
        - RegionMap
        - Ref: AWS::Region
        - AMI
    DependsOn: myDB
  myDB:
    Type: AWS::RDS::DBInstance
    Properties:
      AllocatedStorage: '5'
      DBInstanceClass: db.t2.small
      Engine: MySQL
      EngineVersion: '5.5'
      MasterUsername: MyName
      MasterUserPassword: MyPassword
```
