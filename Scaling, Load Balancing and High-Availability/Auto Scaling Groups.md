# Auto Scaling Groups

Auto Scaling Groups (ASG) are a fundamental feature of Amazon Web Services (AWS) Elastic Compute Cloud (EC2) that enable automatic scaling and self-healing of EC2 instances. ASGs ensure that the desired number of instances is always running, even in the face of fluctuating demand or instance failures. This documentation provides a detailed exploration of ASG, including its core features, scaling policies, integration with other AWS services, and advanced functionalities like lifecycle hooks and health checks.

## Core Concepts of Auto Scaling Groups

### Automatic Scaling and Self-Healing for EC2

Auto Scaling Groups automatically adjust the number of EC2 instances in response to changing demand. This ensures that applications have the right amount of compute capacity at all times. Additionally, ASGs provide self-healing capabilities by replacing unhealthy instances with new ones, maintaining the desired level of availability and performance.

### Launch Templates and Launch Configurations

ASGs use either Launch Templates or Launch Configurations to define the configuration of new instances. Launch Templates are the recommended option as they offer more flexibility, including support for multiple versions and advanced configurations like instance types, AMIs, and network settings.

## Minimum, Desired, and Maximum Size

The Minimum, Desired, and Maximum Size parameters are critical to defining the behavior of an Auto Scaling Group. These parameters ensure that the ASG maintains the appropriate number of instances to handle workload demands while avoiding over-provisioning or under-provisioning.

### Minimum Size

The Minimum Size is the lowest number of instances that the ASG will maintain, even if the workload decreases. This ensures that a baseline level of capacity is always available to handle incoming traffic or requests. For example, if the minimum size is set to 2, the ASG will never scale below 2 instances, even during periods of low demand.

### Desired Capacity

The Desired Capacity is the ideal number of instances that the ASG should maintain. The ASG continuously monitors the state of instances and adjusts the number to match the desired capacity by provisioning or terminating instances as needed. For example, if the desired capacity is set to 5, the ASG will launch or terminate instances to maintain exactly 5 running instances.

### Maximum Size

The Maximum Size is the upper limit on the number of instances that the ASG can launch. This prevents the ASG from scaling beyond a specified threshold, which could lead to excessive costs or resource usage. For example, if the maximum size is set to 10, the ASG will never launch more than 10 instances, even during peak demand.

## Scaling Policies

Scaling policies define how the ASG adjusts the number of instances in response to changes in demand or metrics. Below are the primary types of scaling policies, along with detailed explanations and use cases:

### Manual Scaling

Manual scaling allows administrators to manually adjust the desired capacity of the ASG. This is useful for predictable changes in demand or during maintenance windows.

Use Case: A development team may manually scale up the ASG before deploying a new version of an application to ensure sufficient capacity for testing.

### Scheduled Scaling

Scheduled scaling adjusts the desired capacity based on a predefined schedule. This is ideal for workloads with predictable traffic patterns.

Use Case: An e-commerce platform may scale up before a major sale event and scale down afterward to handle increased traffic during the sale period.

### Dynamic Scaling

Dynamic scaling automatically adjusts the number of instances based on real-time metrics. There are three types of dynamic scaling policies:

| **Policy Type**     | **Description**                                                                    | **Use Case**                                                                     |
| ------------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Simple Scaling**  | Adjusts capacity by a fixed amount when a specified threshold is breached.         | Scaling up by 2 instances when CPU utilization exceeds 70%.                      |
| **Stepped Scaling** | Adjusts capacity in steps, with different thresholds triggering different actions. | Scaling up by 1 instance at 60% CPU, 2 instances at 70%, and 3 instances at 80%. |
| **Target Tracking** | Maintains a specific metric (e.g., CPU utilization) at a target value.             | Maintaining average CPU utilization at 50% by adding or removing instances.      |

Use Case: A web application experiencing variable traffic throughout the day can use target tracking to maintain consistent performance.

### Cooldown Period

The cooldown period is a configurable value (in seconds) that prevents rapid scaling actions. It ensures that the ASG waits for a specified time before initiating another scaling action, allowing time for the system to stabilize.

## Integration with Elastic Load Balancer (ELB)

ASGs seamlessly integrate with Elastic Load Balancers (ELBs). Instances launched by the ASG are automatically added to the ELB target group, and terminated instances are removed. This ensures that traffic is evenly distributed across healthy instances.

### Key Features

- **Automatic Registration**: New instances are automatically registered with the ELB.
- **Health Checks**: The ELB performs health checks on instances and routes traffic only to healthy instances.
- **Cross-Zone Load Balancing**: Ensures traffic is distributed evenly across all Availability Zones (AZs).

### Use Cases

- **High Availability**: Combining ASG with ELB ensures that traffic is routed only to healthy instances, improving application availability.
- **Elasticity**: The ASG can scale out during peak traffic and scale in during low traffic, while the ELB distributes traffic efficiently.

## Auto Scaling Health Checks

ASGs perform health checks to ensure instances are functioning correctly. There are three types of health checks:

| **Health Check Type** | **Description**                                                                              | **Use Case**                                                                                    |
| --------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **EC2 (Default)**     | Checks if the instance is running and not impaired (e.g., stopping, stopped, or terminated). | Ensuring instances are operational and not in a failed state.                                   |
| **ELB**               | Ensures the instance is healthy according to the Elastic Load Balancer.                      | Ensuring instances are serving traffic correctly and meeting application-level health criteria. |
| **Custom**            | Allows an external system to mark instances as healthy or unhealthy.                         | Integrating with third-party monitoring tools to perform custom health checks.                  |

### Health Check Grace Period

The health check grace period is a configurable delay (default: 300 seconds) before ASG starts performing health checks on newly launched instances. This allows time for the instance to complete its initialization process.

Use Case: A web application with a long initialization time can use a grace period of 600 seconds to ensure the instance is fully operational before health checks begin.

## Lifecycle Hooks

Lifecycle hooks allow you to perform custom actions during instance launch or termination transitions. When a lifecycle hook is triggered, the instance is paused, and you can execute scripts or workflows before the ASG continues or abandons the action.

### Key Features of Lifecycle Hooks

- **Pause Instances**: Instances are paused during launch or termination.
- **Timeout**: The hook waits for a specified timeout period before proceeding.
- **Resume Action**: Use the CompleteLifecycleAction API to resume the ASG process.
- **EventBridge or SNS Notifications**: Notifications can be sent to EventBridge or SNS for further processing.

### Use Cases

- **Backup Data**: Backup data from an instance before it is terminated.
- **Custom Configuration**: Perform additional configuration tasks during instance launch.

## Cost Considerations

- **ASG is Free**: There is no additional cost for using Auto Scaling Groups. You only pay for the EC2 instances and other resources created.
- **Granularity**: Using smaller instances can provide better cost efficiency and granularity in scaling.
- **Cool Downs**: Use cooldown periods to avoid rapid scaling, which can lead to unnecessary costs.

## Best Practices

- **Use with ALBs**: Combine ASGs with Application Load Balancers (ALBs) for better elasticity and abstraction.
- **Define WHEN and WHERE**: Use ASGs to define when and where instances are launched, and Launch Templates to define what is launched.
- **Monitor Metrics**: Use CloudWatch metrics to fine-tune scaling policies and ensure optimal performance.
- **Leverage Lifecycle Hooks**: Use lifecycle hooks for custom actions during instance transitions.

## Reference Links

Below are some useful reference links:

- [AWS Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/)
- [Launch Templates vs. Launch Configurations](https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html)
- [Scaling Policies Overview](https://docs.aws.amazon.com/autoscaling/ec2/userguide/scaling_plan.html)
- [Dynamic Scaling Policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html)
- [Integration with Elastic Load Balancer (ELB)](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-load-balancer.html)
- [Auto Scaling Health Checks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/healthcheck.html)
- [Lifecycle Hooks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html)
- [Cooldown Periods](https://docs.aws.amazon.com/autoscaling/ec2/userguide/Cooldown.html)
- [Best Practices for Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-best-practices.html)
- [AWS CloudWatch Metrics for Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-using-cloudwatch.html)
