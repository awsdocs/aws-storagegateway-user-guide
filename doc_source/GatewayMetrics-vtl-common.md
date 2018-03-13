# Monitoring Your Tape Gateway<a name="GatewayMetrics-vtl-common"></a>

In this section, you can find information about how to monitor your tape gateway, virtual tapes associated with your tape gateway, cache storage, and the upload buffer\. You use the AWS Management Console to view metrics for your tape gateway\. With metrics, you can track the health of your tape gateway and set up alarms to notify you when one or more metrics are outside a defined threshold\. 

Storage Gateway provides CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective of how your tape gateway and virtual tapes are performing\. For detailed information about CloudWatch, see the *[Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\. 


+ [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-vtl-common)
+ [Measuring Performance Between Your Tape Gateway and AWS](#PerfGatewayAWS-vtl-common)

## Using Amazon CloudWatch Metrics<a name="UsingCloudWatchConsole-vtl-common"></a>

You can get monitoring data for your tape gateway by using either the AWS Management Console or the CloudWatch API\. The console displays a series of graphs based on the raw data from the CloudWatch API\. The CloudWatch API can also be used through one of the [Amazon AWS Software Development Kits \(SDKs\)](http://aws.amazon.com/code) or the [Amazon CloudWatch API](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/) tools\. Depending on your needs, you might prefer to use either the graphs displayed in the console or retrieved from the API\.

Regardless of which method you choose to use to work with metrics, you must specify the following information: 

+ The metric dimension to work with\. A *dimension* is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Storage Gateway are `GatewayId` and `GatewayName`\. In the CloudWatch console, you can use the `Gateway Metrics` view to easily select gateway\-specific and tape\-specific dimensions\. For more information about dimensions, see [Dimensions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*\.

+ The metric name, such as `ReadBytes`\.

The following table summarizes the types of Storage Gateway metric data that are available to you\. 


| Amazon CloudWatch Namespace | Dimension | Description | 
| --- | --- | --- | 
| AWS/StorageGateway |  GatewayId, GatewayName  |  These dimensions filter for metric data that describes aspects of the tape gateway\. You can identify a tape gateway to work with by specifying both the `GatewayId` and the `GatewayName` dimensions\.  Throughput and latency data of a tape gateway is based on all the virtual tapes in the tape gateway\. Data is available automatically in 5\-minute periods at no charge\.   | 

Working with gateway and tape metrics is similar to working with other service metrics\. You can find a discussion of some of the most common metrics tasks in the CloudWatch documentation listed following:

+ [Viewing Available Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)

+ [Getting Statistics for a Metric](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_GetStatistics.html)

+ [Creating CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

## Measuring Performance Between Your Tape Gateway and AWS<a name="PerfGatewayAWS-vtl-common"></a>

Data throughput, data latency, and operations per second are measures that you can use to understand how your application storage that is using your tape gateway is performing\. When you use the correct aggregation statistic, these values can be measured by using the Storage Gateway metrics that are provided for you\. 

A *statistic* is an aggregation of a metric over a specified period of time\. When you view the values of a metric in CloudWatch, use the `Average` statistic for data latency \(milliseconds\), and use the `Samples` statistic for input/output operations per second \(IOPS\)\. For more information, see [Statistics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistics) in the Amazon CloudWatch User Guide

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

1. Choose **Create Alarm** to start the Create Alarm Wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. Define the alarm by defining the alarm state when the `CloudBytesUploaded` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudBytesUploaded` metric is greater than 10 megabytes for 60 minutes\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\. 

1. Choose **Create Alarm**\.<a name="GatewayAlarm3-vtl-common"></a>

**To set an upper threshold alarm for reading data from AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm Wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the tape gateway that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. Define the alarm by defining the alarm state when the `CloudDownloadLatency` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudDownloadLatency` is greater than 60,000 milliseconds for greater than 2 hours\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.