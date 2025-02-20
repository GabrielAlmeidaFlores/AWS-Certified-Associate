# CodeBuild

AWS CodeBuild is a fully managed continuous integration (CI) service that compiles source code, runs tests, and produces deployable artifacts. Unlike traditional build systems that require manual provisioning and maintenance of build environments, CodeBuild dynamically scales to handle multiple concurrent builds, eliminating the need for dedicated build servers.

One of the key advantages of AWS CodeBuild is its pay-as-you-go pricing model, ensuring that users only pay for the computing resources consumed during the build process. This makes it a cost-effective alternative to traditional on-premises build solutions such as Jenkins, particularly for teams looking to reduce infrastructure overhead.

## Core Functionality

AWS CodeBuild is primarily designed for building and testing software. It can be used as a standalone service or integrated into a larger CI/CD pipeline through AWS CodePipeline. The service supports Docker-based build environments, allowing developers to use predefined build images or create custom ones to match their specific needs.

CodeBuild integrates seamlessly with various AWS services, including AWS Identity and Access Management (IAM) for security, AWS Key Management Service (KMS) for encryption, Amazon Virtual Private Cloud (VPC) for network isolation, AWS CloudTrail for auditing, and Amazon Simple Storage Service (S3) for storing build artifacts.

## Architecture and Workflow

The AWS CodeBuild architecture follows a structured workflow:

- **Source Retrieval**: The build process begins by retrieving source code from a repository such as AWS CodeCommit, GitHub, GitHub Enterprise, Bitbucket, Amazon S3, or AWS CodePipeline.

- **Build Execution**: CodeBuild executes build instructions defined in a `buildspec.yml` file, which is located at the root of the source code repository. The `buildspec.yml` file specifies the build process, including commands, environment variables, and output artifacts.

- **Logging and Monitoring**: Logs from the build process are automatically sent to Amazon S3 and Amazon CloudWatch Logs, providing visibility into build execution.

- **Event Notifications**: Build events can be sent to AWS EventBridge, enabling automated responses or notifications.

- **Artifact Storage**: The final build artifacts are stored in Amazon S3 or optionally published to another service for deployment.

## Build Specification (`buildspec.yml`)

The `buildspec.yml` file is the core configuration file for AWS CodeBuild. It defines how the build process should be executed and consists of four key phases:

- **Install Phase**: This phase is responsible for installing any necessary packages in the build environment.

- **Pre-Build Phase**: Tasks such as signing into external services or installing dependencies occur in this phase.

- **Build Phase**: The primary build commands are executed, including compilation and running tests.

- **Post-Build Phase**: This phase typically includes packaging artifacts, pushing Docker images, or triggering notifications.

Additionally, `buildspec.yml` supports the definition of environment variables. These variables can be:

- Directly defined within the file.

- Derived from shell commands.

- Retrieved from AWS Systems Manager Parameter Store.

- Retrieved from AWS Secrets Manager for secure storage.

Example `buildspec.yml` File:

```YAML
version: 0.2

env:
  variables:
    MY_ENV_VAR: "HelloWorld"
  parameter-store:
    API_KEY: "/my/secure/api-key"
  secrets-manager:
    DB_PASSWORD: "my-secret-db-password"

phases:
  install:
    runtime-versions:
      nodejs: 14
  pre_build:
    commands:
      - npm install
  build:
    commands:
      - npm run build
  post_build:
    commands:
      - echo "Build completed successfully with $MY_ENV_VAR"
artifacts:
  files:
    - '**/*'
  base-directory: build
  discard-paths: no
```

## Supported Environments

AWS CodeBuild provides managed environments for various programming languages and frameworks, including but not limited to:

- Java
- Ruby
- Python
- Node.js
- PHP
- .NET Core
- Go
- Docker

Custom environments can also be created using Docker images stored in Amazon Elastic Container Registry (ECR) or Docker Hub.

## Build Artifacts and Logging

Build artifacts, which are the output of the build process, are stored in Amazon S3. Logs generated during the build process can be streamed to Amazon CloudWatch Logs for real-time monitoring and troubleshooting.

## Integration with AWS Services

AWS CodeBuild integrates with several AWS services to enhance its capabilities:

- AWS IAM: Provides fine-grained permissions for accessing repositories and executing builds.

- AWS KMS: Encrypts build artifacts and secrets.

- Amazon S3: Stores build artifacts.

- Amazon CloudWatch Logs: Captures build logs.

- AWS EventBridge: Triggers notifications based on build events.

- AWS CodePipeline: Automates the CI/CD workflow by integrating CodeBuild into deployment pipelines.

## Reference Links

Below are some useful reference links:

- [AWS CodeBuild Documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html)
- [AWS CodeBuild Pricing](https://aws.amazon.com/codebuild/pricing/)
- [AWS CodeBuild Buildspec Reference](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
- [AWS CodeBuild Environment Variables](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html)
- [AWS CodeBuild Supported Runtimes](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-available.html)
- [AWS CodeBuild Logs in CloudWatch](https://docs.aws.amazon.com/codebuild/latest/userguide/logging-cloudwatch.html)
- [AWS CodeBuild Integration with CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/integrations-codebuild.html)
- [AWS CodeBuild EventBridge Integration](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-build-notifications.html)
- [AWS CodeBuild Security and IAM](https://docs.aws.amazon.com/codebuild/latest/userguide/auth-and-access-control.html)
- [AWS CodeBuild Custom Docker Images](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker-custom-image.html)
