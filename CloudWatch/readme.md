# CloudWatch
#### think of a gatekeeper for aws which is basically for monitoring, alerting , reporting and logging mechanism to keep track of aws and it's services

#  Core Components of CloudWatch
### a) Metrics
Numerical data points collected over time for AWS resources or custom applications.
Each metric belongs to a namespace (e.g., AWS/EC2, AWS/S3).
Metrics have dimensions to differentiate data (e.g., InstanceId for EC2).
Examples of AWS Metrics:

#### EC2: CPUUtilization, NetworkIn, DiskReadOps
#### S3: NumberOfObjects, BucketSizeBytes
#### Lambda: Invocations, Errors
### b) Logs
Collect, store, and manage logs from AWS services, applications, or custom sources.
Logs are organized in log groups and log streams:
Log Group: Collection of log streams with common retention and permissions.
Log Stream: Sequence of log events from a specific source.
### c) Alarms
Monitors metrics and performs actions when thresholds are breached.
Types of actions:
Notify via Amazon SNS (email, SMS, etc.).
Execute AWS Lambda functions.
Trigger Auto Scaling policies.
### d) Events (EventBridge)
Monitors changes in the state of AWS resources or applications.
Enables real-time responses to events (e.g., failed API call or EC2 instance launch).
### e) Dashboards
Customizable, interactive views of metrics and logs across AWS accounts and regions.
Supports widgets like line graphs, pie charts, and text.

# Key Features of CloudWatch
### Metrics Monitoring:
Tracks performance metrics (e.g., CPU utilization, memory usage, disk I/O) from AWS services and custom applications.

### Log Management:
Centralized storage, monitoring, and analysis of application, system, and custom logs.

### Alarms:
Set thresholds on metrics to trigger alerts or automated actions (e.g., scaling, Lambda execution).

### Dashboards:
Create custom visualizations of metrics and logs for a consolidated view of your environment.

### Events:
Detect and respond to state changes in resources or applications using Amazon EventBridge (formerly CloudWatch Events).

### Insights:
Analyze logs with CloudWatch Logs Insights to identify trends, errors, and bottlenecks.

### Service Lens:
Provides observability for distributed applications with traces, metrics, and logs.

# Monitoring with CloudWatch
## a) For EC2 Instances
Default Metrics: CPU utilization, disk I/O, network I/O.
Custom Metrics: Memory usage, disk space (requires CloudWatch Agent).
## b) For Serverless Applications (e.g., Lambda)
Monitors invocations, errors, duration, and concurrency.
Logs function execution details.
## c) For Containers (e.g., EKS/ECS)
Monitors container performance metrics like CPU and memory usage.
Collects logs from containerized applications.


# use case
Example Scenarios
## a) Detecting High CPU Utilization
Set an alarm for CPUUtilization > 80%.
Trigger an Auto Scaling action to add another EC2 instance.
## b) Monitoring a Web Application
Use ALB metrics like RequestCount and HTTPCode_ELB_5XX_Count.
Combine with Lambda logs to debug backend errors.
## c) Analyzing Failed API Calls
Use CloudWatch Logs Insights to search for error patterns:
sql

> fields @timestamp, @message
> | filter @message like /error/
> | sort @timestamp desc

