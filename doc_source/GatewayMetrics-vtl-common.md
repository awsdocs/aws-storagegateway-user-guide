# Monitoring Your Tape Gateway<a name="GatewayMetrics-vtl-common"></a>

In this section, you can find information about how to monitor your tape gateway, virtual tapes associated with your tape gateway, cache storage, and the upload buffer\. You use the AWS Management Console to view metrics for your tape gateway\. With metrics, you can track the health of your tape gateway and set up alarms to notify you when one or more metrics are outside a defined threshold\. 

Storage Gateway provides CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective of how your tape gateway and virtual tapes are performing\. For detailed information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Topics**
+ [Getting Tape Gateway Health Logs with CloudWatch Log Groups](#cw-log-groups-tape)
+ [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-vtl-common)
+ [Understanding Virtual Tape Metrics](#monitoring-tape)
+ [Measuring Performance Between Your Tape Gateway and AWS](#PerfGatewayAWS-vtl-common)

## Getting Tape Gateway Health Logs with CloudWatch Log Groups<a name="cw-log-groups-tape"></a>

You can use Amazon CloudWatch Logs to get information about the health of your tape gateway and related resources\. You can use the logs to monitor your gateway for errors that it encounters\. In addition, you can use Amazon CloudWatch subscription filters to automate processing of the log information in real time\. For more information, see [Real\-time Processing of Log Data with Subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) in the *Amazon CloudWatch User Guide\.*

For example, suppose that your gateway is deployed in a cluster enabled with VMware HA and you need to know about any errors\. You can configure a CloudWatch log group to monitor your gateway and get notified when your gateway encounters an error\. You can either configure the group when you are activating the gateway or after your gateway is activated and up and running\. For information about how to configure a CloudWatch log group when activating a gateway, see [Configuring Amazon CloudWatch Logging](create-gateway-vtl.md#configure-logging-tape)\. For general information about CloudWatch log groups, see [Working with Log Groups and Log Streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch User Guide\.*

For information about how to troubleshoot and fix these types of errors, see [Troubleshooting virtual tape issues](Main_TapesIssues-vtl.md)\.

The following procedure shows you how to configure a CloudWatch log group after your gateway is activated\. 

**To configure a CloudWatch log group to work with your tape gateway**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways** and choose the gateway that you want to configure the CloudWatch log group for\. 

1. For **Actions**, choose **Edit gateway Information**\.

1. For **Gateway Log Group**, choose the log group that you want to use\. If you don't have a log group, choose the **Create new Log Group** link to create one\. You are directed to the CloudWatch Logs console where you can create the log group\. If you create a new log group, choose the refresh button to view the new log group in the list\.

1. When you are done, choose **Save**\.

Following is an example of a tape gateway event message that is sent to CloudWatch\. This example shows a `TapeStatusTransition` message\.

```
    {
    "severity": "INFO",
    "source": "FZTT16FCF5",
    "type": "TapeStatusTransition",
    "gateway": "sgw-C51DFEAC",
    "timestamp": "1581553463831",
    "newStatus": "RETRIEVED"
    }
```

To see the logs for your gateway, choose the gateway and choose the **Details** tab\.

## Using Amazon CloudWatch Metrics<a name="UsingCloudWatchConsole-vtl-common"></a>

You can get monitoring data for your tape gateway by using either the AWS Management Console or the CloudWatch API\. The console displays a series of graphs based on the raw data from the CloudWatch API\. The CloudWatch API can also be used through one of the [Amazon AWS Software Development Kits \(SDKs\)](http://aws.amazon.com/tools) or the [Amazon CloudWatch API](http://aws.amazon.com/cloudwatch) tools\. Depending on your needs, you might prefer to use either the graphs displayed in the console or retrieved from the API\.

Regardless of which method you choose to use to work with metrics, you must specify the following information: 
+ The metric dimension to work with\. A *dimension* is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Storage Gateway are `GatewayId` and `GatewayName`\. In the CloudWatch console, you can use the `Gateway Metrics` view to easily select gateway\-specific and tape\-specific dimensions\. For more information about dimensions, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*\.
+ The metric name, such as `ReadBytes`\.

The following table summarizes the types of Storage Gateway metric data that are available to you\. 


| Amazon CloudWatch Namespace | Dimension | Description | 
| --- | --- | --- | 
| AWS/StorageGateway |  GatewayId, GatewayName  |  These dimensions filter for metric data that describes aspects of the tape gateway\. You can identify a tape gateway to work with by specifying both the `GatewayId` and the `GatewayName` dimensions\.  Throughput and latency data of a tape gateway is based on all the virtual tapes in the tape gateway\. Data is available automatically in 5\-minute periods at no charge\.   | 

Working with gateway and tape metrics is similar to working with other service metrics\. You can find a discussion of some of the most common metrics tasks in the CloudWatch documentation listed following:
+ [Viewing Available Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)
+ [Getting Statistics for a Metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_GetStatistics.html)
+ [Creating CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

## Understanding Virtual Tape Metrics<a name="monitoring-tape"></a>

You can find information following about the Storage Gateway metrics that cover virtual tapes\. Each tape has a set of metrics associated with it\. 

Some tape\-specific metrics might have the same name as certain gateway\-specific metrics\. These metrics represent the same kinds of measurements but are scoped to a tape instead of a gateway\. Before starting work, specify whether you want to work with a gateway metric or a tape metric\. When working with tape metrics, specify the tape ID for the tape that you want to view metrics for\. For more information, see [Using Amazon CloudWatch Metrics](monitoring-volume-gateway.md#UsingCloudWatchConsole-common)\.

The following table describes the Storage Gateway metrics that you can use to get information about your tapes\. 


| Metric | Description | 
| --- | --- | 
| CachePercentDirty |  The tape's contribution to the overall percentage of the gateway's cache that isn't persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that isn't persisted to AWS\. For more information, see [Understanding gateway metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | 
| ClientTraffic |  The amount of bytes the tape sent and received from on\-premises clients\. That is, the total amount of `ReadBytes` and `WriteBytes` from client applications\. Units: bytes  | 
| CloudTraffic |  The amount of bytes uploaded and downloaded from the cloud to the tape\. Units: bytes  | 
| CpuUsage |  The percentage of allocated CPU compute units that are currently used by the tape\.  Units: Percent  | 
| HealthNotificationCount |  The number of health notifications sent by the tape\. Units: count  | 
| MemoryUsage |  The percentage of allocated memory that is currently used by the tape\.  Units: Percent  | 

## Measuring Performance Between Your Tape Gateway and AWS<a name="PerfGatewayAWS-vtl-common"></a>

Data throughput, data latency, and operations per second are measures that you can use to understand how your application storage that is using your tape gateway is performing\. When you use the correct aggregation statistic, these values can be measured by using the Storage Gateway metrics that are provided for you\. 

A *statistic* is an aggregation of a metric over a specified period of time\. When you view the values of a metric in CloudWatch, use the `Average` statistic for data latency \(milliseconds\), and use the `Samples` statistic for input/output operations per second \(IOPS\)\. For more information, see [Statistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistic) in the *Amazon CloudWatch User Guide*\.

The following table summarizes the metrics and the corresponding statistic you can use to measure the throughput, latency, and IOPS between your tape gateway and AWS\. 


| Item of Interest | How to Measure | 
| --- | --- | 
| Latency | Use the ReadTime and WriteTime metrics with the Average CloudWatch statistic\. For example, the Average value of the ReadTime metric gives you the latency per operation over the sample period of time\.  | 
| Throughput to AWS | Use the CloudBytesDownloaded and CloudBytesUploaded metrics with the Sum CloudWatch statistic\. For example, the Sum value of the CloudBytesDownloaded metric over a sample period of 5 minutes divided by 300 seconds gives you the throughput from AWS to the tape gateway as a rate in bytes per second\. | 
| Latency of data to AWS | Use the CloudDownloadLatency metric with the Average statistic\. For example, the Average statistic of the CloudDownloadLatency metric gives you the latency per operation\. | 

**To measure the upload data throughput from a tape gateway to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **Metrics** tab\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Sum` statistic\.

1. For **Period**, choose a value of 5 minutes or greater\.

1. In the resulting time\-ordered set of data points, divide each data point by the period \(in seconds\) to get the throughput at that sample period\.

The following image shows the `CloudBytesUploaded` metric for a gateway tape with the `Sum` statistic\. In the image, placing the cursor over a data point displays information about the data point, including its value and the number of bytes uploaded\. Divide this value by the **Period** value \(5 minutes\) to get the throughput at that sample point\. For the point highlighted, the throughput from the tape gateway to AWS is 555,544,576 bytes divided by 300 seconds, which is 1\.7 megabytes per second\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_25.png)

**To measure the data latency from a tape gateway to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **Metrics** tab\.

1. Choose the **StorageGateway: GatewayMetrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

 The resulting time\-ordered set of data points contains the latency in milliseconds\.<a name="GatewayAlarm2-vtl-common"></a>

**To set an upper threshold alarm for a tape gateway's throughput to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. Define the alarm by defining the alarm state when the `CloudBytesUploaded` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudBytesUploaded` metric is greater than 10 megabytes for 60 minutes\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\. 

1. Choose **Create Alarm**\.<a name="GatewayAlarm3-vtl-common"></a>

**To set an upper threshold alarm for reading data from AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. Define the alarm by defining the alarm state when the `CloudDownloadLatency` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudDownloadLatency` is greater than 60,000 milliseconds for greater than 2 hours\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.