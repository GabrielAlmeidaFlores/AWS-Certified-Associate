# EventBridge

Amazon EventBridge is a serverless event bus service that enables you to build event-driven applications by connecting various AWS services and SaaS applications. It allows you to ingest, process, and route real-time data from multiple sources to targets such as AWS Lambda, SNS, SQS, and more. EventBridge simplifies the process of building scalable and decoupled systems by providing a centralized way to manage events.

## Key Concepts

### Event Bus

An event bus is a pipeline that receives events from various sources and routes them to the appropriate targets. EventBridge provides a default event bus for AWS services, and you can create custom event buses for your applications. Custom event buses allow you to isolate events for specific applications or environments.

### Events

An event is a JSON object that describes a change in state within a system. Events are composed of a set of key-value pairs, including metadata such as the source, detail-type, and detail. The `source` field identifies the service or application that generated the event, the `detail-type` specifies the type of event, and the `detail` contains the event-specific data.

### Source

In Amazon EventBridge, the source is a key attribute of an event that identifies the origin or producer of the event. It is a string value that helps categorize and filter events based on where they come from. For example, the source could be an AWS service (e.g., `aws.ec2` for Amazon EC2 events), a custom application, or a third-party SaaS provider.

When creating an event pattern rule, the source is one of the primary fields you can use to filter and route events. By specifying a source in the event pattern, you ensure that only events originating from that particular source are matched and sent to the associated targets. This allows for precise control over event routing and processing in your event-driven architecture.

### Rules

In Amazon EventBridge, rules determine how events are routed to targets. There are two primary types of rules you can create:

- **Event Pattern Rules**: These rules use an event pattern, which is a JSON object that specifies the criteria for matching events. When an event matches the defined pattern, it is sent to the associated targets. Event patterns allow you to filter events based on specific attributes, such as the event source, detail-type, or other custom fields.

- **Schedule Rules**: These rules allow you to trigger targets at specific times or intervals, similar to a cron job. Instead of matching incoming events, schedule rules are based on a predefined schedule, enabling you to automate tasks or workflows at regular intervals or specific points in time.

Both types of rules can be associated with one or more targets, such as AWS Lambda functions, Amazon SNS topics, or other AWS services, to execute actions when the rule is triggered. This flexibility makes EventBridge a powerful tool for event-driven architectures and scheduled automation.

### Targets

Targets are the resources that process events. EventBridge supports a wide range of targets, including:

- AWS Lambda functions
- Amazon SNS topics
- Amazon SQS queues
- AWS Step Functions
- Amazon Kinesis Data Streams
- AWS API Gateway
- And more.

Each target can be configured with additional settings, such as input transformations, which allow you to modify the event data before it is sent to the target. This flexibility ensures that events can be tailored to meet the specific requirements of the target resource.

## Architecture Example

Below is an example architecture diagram that demonstrates how Amazon EventBridge can be used to build an event-driven application:

<img src="https://docs.aws.amazon.com/images/eventbridge/latest/userguide/images/bus-overview-types_eventbridge_conceptual.svg">

### Architecture Breakdown

- **Event Sources**: The diagram illustrates three main event sources—AWS services, custom applications, and SaaS applications. AWS services generate events that are automatically sent to the default event bus. Custom applications send their events to a custom event bus, while SaaS applications use a partner event source that directs their events to a partner event bus.

- **Event Buses**: Once an event reaches an event bus, it is classified based on its source. The default event bus is dedicated to AWS service events, whereas custom event buses provide flexibility for managing application-specific events. The partner event bus ensures seamless integration for external SaaS providers.

- **Rules Evaluation**: Events arriving at an event bus are evaluated against predefined rules. These rules contain filtering logic to determine whether an event should be processed further. If an event matches the conditions specified in a rule, it is forwarded to one or more targets. If it does not match any rule, it is discarded.

- **Target Processing**: Successfully matched events are sent to targets, which can include AWS services such as Lambda, SQS, SNS, Step Functions, or other integrations. These targets process the events based on business logic and trigger appropriate workflows. Events that do not meet any rule conditions are not processed further.

## Event Schema

EventBridge uses a schema to define the structure of events. Schemas are JSON documents that describe the fields and data types of an event. EventBridge provides a schema registry where you can discover, manage, and version schemas for AWS services and custom applications. Schemas can be used to validate events and generate code bindings for various programming languages.

### Schema Registry

The schema registry is a centralized repository for managing event schemas. It allows you to discover schemas for AWS services, create custom schemas, and version schemas as they evolve. The schema registry also provides tools for generating code bindings, which can simplify the process of working with events in your applications.

### Schema Discovery

EventBridge can automatically discover schemas for events that are sent to an event bus. This feature is useful for understanding the structure of events generated by AWS services or third-party applications. Once a schema is discovered, it can be added to the schema registry and used to validate events or generate code bindings.

## Event Patterns

Event patterns are used to filter events based on specific criteria. They are defined as JSON objects that specify the fields and values to match. Event patterns can include simple comparisons, such as equality checks, as well as more complex conditions, such as wildcard matching or numeric ranges.

Example Event Pattern:

```JSON
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["stopped"]
  }
}
```

This pattern matches events where the source is `aws.ec2`, the detail-type is `EC2 Instance State-change Notification`, and the state in the detail is `stopped`.

## Input Transformation

Input transformation allows you to modify the event data before it is sent to a target. This is useful for converting the event into a format that is compatible with the target or for extracting specific fields from the event. Input transformations are defined using JSONPath expressions, which allow you to select and manipulate parts of the event data.

Example Input Transformation:

```JSON
{
  "instance-id": "$.detail.instance-id",
  "state": "$.detail.state",
  "timestamp": "$.time"
}
```

This transformation extracts the `instance-id`, `state`, and `timestamp` fields from the event and sends them to the target.

## EventBridge Pricing

EventBridge pricing is based on the number of events ingested and the number of rules evaluated. There are no upfront costs or minimum fees, and you only pay for what you use. The following table provides an overview of EventBridge pricing:

| Metric           | Price (USD)       |
| ---------------- | ----------------- |
| Events Ingested  | $1.00 per million |
| Rules Evaluated  | $1.00 per million |
| Custom Event Bus | $1.00 per million |
| Schema Discovery | $0.10 per million |
| Schema Registry  | $0.10 per schema  |

## Use Cases

- **Real-Time Data Processing**: EventBridge can be used to process real-time data from various sources, such as IoT devices, application logs, or user interactions. By routing events to targets like AWS Lambda or Amazon Kinesis, you can build real-time data processing pipelines that scale automatically with your workload.

- **Microservices Orchestration**: EventBridge is well-suited for orchestrating microservices in a decoupled architecture. By using events to communicate between services, you can reduce dependencies and improve the scalability and resilience of your system. Each microservice can subscribe to the events it needs and process them independently.

- **SaaS Integration**: EventBridge supports integration with third-party SaaS applications, such as Zendesk, Datadog, and PagerDuty. This allows you to ingest events from these applications and route them to AWS services for further processing. For example, you can use EventBridge to trigger an AWS Lambda function when a new support ticket is created in Zendesk.

## Best Practices

- **Use Custom Event Buses**: Custom event buses allow you to isolate events for specific applications or environments. This can help you manage events more effectively and reduce the risk of interference between different parts of your system. For example, you can create separate event buses for development, testing, and production environments.

- **Monitor and Log Events**: Monitoring and logging are essential for understanding the behavior of your event-driven applications. EventBridge integrates with Amazon CloudWatch, allowing you to monitor event delivery and rule execution. You can also log events to Amazon S3 or Amazon CloudWatch Logs for long-term storage and analysis.

- **Optimize Event Patterns**: Event patterns should be optimized to match only the events that are relevant to your application. This can help reduce the number of events processed by your targets and improve the efficiency of your system. Avoid using overly broad patterns that match a large number of events, as this can lead to unnecessary processing and increased costs.
