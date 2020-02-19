# Monitoring Your Volume Gateway<a name="monitoring-volume-gateway"></a>

In this section, you can find information about how to monitor a gateway in a cached volumes or stored volumes setup, including monitoring the volumes associated with the gateway and monitoring the upload buffer\. You use the AWS Management Console to view metrics for your gateway\. For example, you can view the number of bytes used in read and write operations, the time spent in read and write operations, and the time taken to retrieve data from the AWS cloud\. With metrics, you can track the health of your gateway and set up alarms to notify you when one or more metrics fall outside a defined threshold\. 

Storage Gateway provides CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective on how your gateway and volumes are performing\. For detailed information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Topics**
+ [Getting Volume Gateway Health Logs with CloudWatch Log Groups](#cw-log-groups-volume)
+ [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-common)
+ [Measuring Performance Between Your Application and Gateway](#PerfAppGateway-common)
+ [Measuring Performance Between Your Gateway and AWS](#PerfGatewayAWS-common)
+ [Understanding Volume Metrics](#MonitoringVolumes-common)

## Getting Volume Gateway Health Logs with CloudWatch Log Groups<a name="cw-log-groups-volume"></a>

You can use Amazon CloudWatch Logs to get information about the health of your volume gateway and related resources\. You can use the logs to monitor your gateway for errors that it encounters\. In addition, you can use Amazon CloudWatch subscription filters to automate processing of the log information in real time\. For more information, see [Real\-time Processing of Log Data with Subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) in the *Amazon CloudWatch User Guide\.*

For example, suppose that your gateway is deployed in a cluster enabled with VMware HA and you need to know about any errors\. You can configure a CloudWatch log group to monitor your gateway and get notified when your gateway encounters an error\. You can either configure the group when you are activating the gateway or after your gateway is activated and up and running\. For information about how to configure a CloudWatch log group when activating a gateway, see [Configuring Amazon CloudWatch Logging](create-volume-gateway.md#configure-logging-volume)\. For general information about CloudWatch log groups, see [Working with Log Groups and Log Streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch User Guide\.*

 For information about how to troubleshoot and fix these types of errors, see [Troubleshooting File Gateway Issues](troubleshoot-logging-errors.md)\.

The following procedure shows you how to configure a CloudWatch log group after your gateway is activated\. 

**To configure a CloudWatch log group to work with your volume gateway**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways** and choose the gateway that you want to configure the CloudWatch log group for\. 

1. For **Actions**, choose **Edit gateway Information**\.

1. For **Gateway Log Group**, choose the log group that you want to use\. If you don't have a log group, choose the **Create new Log Group** link to create one\. You are directed to the CloudWatch Logs console where you can create the log group\. If you create a new log group, choose the refresh button to view the new log group in the list\.

1. When you are done, choose **Save**\.

To see the logs for your gateway, choose the gateway and choose the **Details** tab\.

## Using Amazon CloudWatch Metrics<a name="UsingCloudWatchConsole-common"></a>

You can get monitoring data for your gateway using either the AWS Management Console or the CloudWatch API\. The console displays a series of graphs based on the raw data from the CloudWatch API\. You can also use the CloudWatch API through one of the [Amazon AWS Software Development Kits \(SDKs\)](http://aws.amazon.com/tools) or the [Amazon CloudWatch API](http://aws.amazon.com/cloudwatch) tools\. Depending on your needs, you might prefer to use either the graphs displayed in the console or retrieved from the API\. 

Regardless of which method you choose to use to work with metrics, you must specify the following information: 
+ The metric dimension to work with\. A *dimension* is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Storage Gateway are `GatewayId`, `GatewayName`, and `VolumeId`\. In the CloudWatch console, you can use the `Gateway Metrics` and `Volume Metrics` views to easily select gateway\-specific and volume\-specific dimensions\. For more information about dimensions, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*\.
+ The metric name, such as `ReadBytes`\.

The following table summarizes the types of Storage Gateway metric data that you can use\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/monitoring-volume-gateway.html)

Working with gateway and volume metrics is similar to working with other service metrics\. You can find a discussion of some of the most common metrics tasks in the CloudWatch documentation listed following: 
+ [Viewing Available Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)
+ [Getting Statistics for a Metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_GetStatistics.html)
+ [Creating CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

## Measuring Performance Between Your Application and Gateway<a name="PerfAppGateway-common"></a>

Data throughput, data latency, and operations per second are three measures that you can use to understand how your application storage that is using your gateway is performing\. When you use the correct aggregation statistic, you can use Storage Gateway metrics to measure these values\. 

A *statistic* is an aggregation of a metric over a specified period of time\. When you view the values of a metric in CloudWatch, use the `Average` statistic for data latency \(milliseconds\), use the `Sum` statistic for data throughput \(bytes per second\), and use the `Samples` statistic for input/output operations per second \(IOPS\)\. For more information, see [Statistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistic) in the *Amazon CloudWatch User Guide*\.

The following table summarizes the metrics and corresponding statistic you can use to measure the throughput, latency, and IOPS between your applications and gateways\. 


| Item of Interest | How to Measure | 
| --- | --- | 
| Throughput  |  Use the `ReadBytes` and `WriteBytes` metrics with the `Sum` CloudWatch statistic\. For example, the `Sum` value of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the throughput as a rate in bytes per second\.  | 
| Latency | Use the ReadTime and WriteTime metrics with the Average CloudWatch statistic\. For example, the Average value of the ReadTime metric gives you the latency per operation over the sample period of time\. | 
| IOPS | Use the ReadBytes and WriteBytes metrics with the Samples CloudWatch statistic\. For example, the Samples value of the ReadBytes metric over a sample period of 5 minutes divided by 300 seconds gives you IOPS\. | 

For the average latency graphs and average size graphs, the average is calculated over the total number of operations \(read or write, whichever is applicable to the graph\) that completed during the period\. 

**To measure the data throughput from an application to a volume**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose the **All metrics** tab and then choose **Storage Gateway**\.

1. Choose the **Volume metrics** dimension, and find the volume that you want to work with\.

1. Choose the `ReadBytes` and `WriteBytes` metrics\.

1. For **Time Range**, choose a value\.

1. Choose the `Sum` statistic\.

1. For **Period**, choose a value of 5 minutes or greater\.

1. In the resulting time\-ordered sets of data points \(one for `ReadBytes` and one for `WriteBytes`\), divide each data point by the period \(in seconds\) to get the throughput at the sample point\. The total throughput is the sum of the throughputs\.

The following image shows the `ReadBytes` and `WriteBytes` metrics for a volume with the `Sum` statistic\. In the image, the cursor over a data point displays information about the data point including its value and the number of bytes\. Divide the bytes value by the **Period** value \(5 minutes\) to get the data throughput at that sample point\. For the point highlighted, the read throughput is 2,384,199,680 bytes divided by 300 seconds, which is 7\.6 megabytes per second\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_15.png)

**To measure the data input/output operations per second from an application to a volume**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose the **All metrics** tab and then choose **Storage Gateway**\.

1. Choose the **Volume metrics** dimension, and find the volume that you want to work with\.

1. Choose the `ReadBytes` and `WriteBytes` metrics\.

1. For **Time Range**, choose a value\.

1. Choose the `Samples` statistic\.

1. For **Period**, choose a value of 5 minutes or greater\.

1. In the resulting time\-ordered sets of data points \(one for `ReadBytes` and one for `WriteBytes`\), divide each data point by the period \(in seconds\) to get IOPS\.

The following image shows the `ReadBytes` and `WriteBytes` metrics for a storage volume with the `Samples` statistic\. In the image, the cursor over a data point displays information about the data point, including its value and the number of samples\. Divide the samples value by the **Period** value \(5 minutes\) to get the operations per second at that sample point\. For the point highlighted, the number of write operations is 24,373 bytes divided by 300 seconds, which is 81 write operations per second\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_20.png)

## Measuring Performance Between Your Gateway and AWS<a name="PerfGatewayAWS-common"></a>

Data throughput, data latency, and operations per second are three measures that you can use to understand how your application storage using the Storage Gateway is performing\. These three values can be measured using the Storage Gateway metrics provided for you when you use the correct aggregation statistic\. The following table summarizes the metrics and corresponding statistic to use to measure the throughput, latency, and input/output operations per second \(IOPS\) between your gateway and AWS\. 


| Item of Interest | How to Measure | 
| --- | --- | 
| Throughput  |  Use the `ReadBytes` and `WriteBytes` metrics with the `Sum` CloudWatch statistic\. For example, the `Sum` value of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the throughput as a rate in bytes per second\.   | 
| Latency | Use the ReadTime and WriteTime metrics with the Average CloudWatch statistic\. For example, the Average value of the ReadTime metric gives you the latency per operation over the sample period of time\.  | 
| IOPS | Use the ReadBytes and WriteBytes metrics with the Samples CloudWatch statistic\. For example, the Samples value of the ReadBytes metric over a sample period of 5 minutes divided by 300 seconds gives you IOPS\.  | 
| Throughput to AWS | Use the CloudBytesDownloaded and CloudBytesUploaded metrics with the Sum CloudWatch statistic\. For example, the Sum value of the CloudBytesDownloaded metric over a sample period of 5 minutes divided by 300 seconds gives you the throughput from AWS to the gateway as bytes per second\. | 
| Latency of data to AWS | Use the CloudDownloadLatency metric with the Average statistic\. For example, the Average statistic of the CloudDownloadLatency metric gives you the latency per operation\. | 

**To measure the upload data throughput from a gateway to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose the **All metrics** tab and then choose **Storage Gateway**\.

1. Choose the **Gateway metrics** dimension, and find the volume that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Sum` statistic\.

1. For **Period**, choose a value of 5 minutes or greater\.

1. In the resulting time\-ordered set of data points, divide each data point by the period \(in seconds\) to get the throughput at that sample period\.

The following image shows the `CloudBytesUploaded` metric for a gateway volume with the `Sum` statistic\. In the image, the cursor over a data point displays information about the data point, including its value and bytes uploaded\. Divide this value by the **Period** value \(5 minutes\) to get the throughput at that sample point\. For the point highlighted, the throughput from the gateway to AWS is 555,544,576 bytes divided by 300 seconds, which is 1\.7 megabytes per second\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_25.png)

**To measure the latency per operation of a gateway**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose the **All metrics** tab and then choose **Storage Gateway**\.

1. Choose the **Gateway metrics** dimension, and find the volume that you want to work with\.

1. Choose the `ReadTime` and `WriteTime` metrics\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

1.  In the resulting time\-ordered set of points \(one for `ReadTime` and one for `WriteTime`\), add data points at the same time sample to get to the total latency in milliseconds\.

**To measure the data latency from a gateway to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose the **All metrics** tab and then choose **Storage Gateway**\.

1. Choose the **Gateway metrics** dimension, and find the volume that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the latency in milliseconds\.<a name="GatewayAlarm2-common"></a>

**To set an upper threshold alarm for a gateway's throughput to AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Alarms**\.

1. Choose **Create Alarm** to start the Create Alarm wizard\.

1. Choose the **Storage Gateway** dimension, and find the gateway that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. To define the alarm, define the alarm state when the `CloudBytesUploaded` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudBytesUploaded` metric is greater than 10 MB for 60 minutes\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.<a name="GatewayAlarm3-common"></a>

**To set an upper threshold alarm for reading data from AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. Define the alarm by defining the alarm state when the `CloudDownloadLatency` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudDownloadLatency` is greater than 60,000 milliseconds for greater than 2 hours\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.

## Understanding Volume Metrics<a name="MonitoringVolumes-common"></a>

You can find information following about the Storage Gateway metrics that cover a volume of a gateway\. Each volume of a gateway has a set of metrics associated with it\. 

Some volume\-specific metrics have the same name as certain gateway\-specific metrics\. These metrics represent the same kinds of measurements but are scoped to the volume instead of the gateway\. Before starting work, specify whether you want to work with a gateway metric or a volume metric\. Specifically, when working with volume metrics, specify the volume ID for the storage volume that you want to view metrics for\. For more information, see [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-common)\.

The following table describes the Storage Gateway metrics that you can use to get information about your storage volumes\. 


| Metric | Description | Cached Volumes | Stored Volumes | 
| --- | --- | --- | --- | 
| CacheHitPercent |  Percent of application read operations from the volume that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the volume, this metric reports 100 percent\.  Units: Percent  | Yes | No | 
| CachePercentDirty |  The volume's contribution to the overall percentage of the gateway's cache that isn't persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that isn't persisted to AWS\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | Yes | Yes | 
| CachePercentUsed |  The volume's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | Yes | No | 
| ClientTraffic |  The number of bytes that the tape sent and received from on\-premises clients\. That is, the total amount of `ReadBytes` and `WriteBytes` from client applications\. Units: Bytes  | Yes | Yes | 
| CloudTraffic |  The number of bytes uploaded and downloaded from the cloud to the volume\. Units: Bytes  | Yes | Yes | 
| CPUUsage |  The percentage of allocated CPU compute units that are currently used by the volume\.  Units: Percent  | Yes | Yes | 
| HealthNotificationCount |  The number of health notifications sent by the volume\. Units: count  | Yes | Yes | 
| MemoryUsage |  The percentage of allocated memory that is currently used by the volume\.  Units: Percent  | Yes | No | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples`statistic to measure IOPS\. Units: Bytes  | Yes | Yes | 
| ReadTime |  The total number of milliseconds spent on read operations from your on\-premises applications in the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | Yes | Yes | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | Yes | Yes | 
| WriteTime |  The total number of milliseconds spent on write operations from your on\-premises applications in the reporting period\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | Yes | Yes | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period\.  Units: Bytes  | Yes | Yes | 