# Module 4

## Introduction to Amazon CloudWatch
- CloudWatch acts as one centralized place where metrics are gathered and analyzed. 
- For applications running on EC2 instances, you can get more granularity by posting metrics every minute instead of every 5 minutes using a feature like detailed monitoring.
- Each metric in CloudWatch has a timestamp and is organized into containers called namespaces. 
- CloudWatch can also be the centralized place for logs to be stored and analyzed, using CloudWatch Logs. CloudWatch Logs can monitor, store, and access your log files from applications running on Amazon EC2 instances, AWS Lambda functions, and other sources.  
    - **Log event:** A log event is a record of activity recorded by the application or resource being monitored, and it has a timestamp and an event message.  
    - **Log stream:** Log events are then grouped into log streams, which are sequences of log events that all belong to the same resource being monitored. For example, logs for an EC2 instance are grouped together into a log stream that you can then filter or query for insights.  
    - **Log groups:** Log streams are then organized into log groups. A log group is composed of log streams that all share the same retention and permissions settings. For example, if you have multiple EC2 instances hosting your application and you are sending application log data to CloudWatch Logs, you can group the log streams from each instance into one log group. This helps keep your logs organized.
