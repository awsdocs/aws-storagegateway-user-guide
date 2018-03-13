# Monitoring Your Volume Gateway<a name="GatewayMetrics-common"></a>

In this section, you can find information about how to monitor a gateway in a cached volumes or stored volumes setup, including monitoring the volumes associated with the gateway and monitoring the upload buffer\. You use the AWS Management Console to view metrics for your gateway\. For example, you can view the number of bytes used in read and write operations, the time spent in read and write operations, and the time taken to retrieve data from the AWS cloud\. With metrics, you can track the health of your gateway and set up alarms to notify you when one or more metrics fall outside a defined threshold\. 


+ [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-common)
+ [Measuring Performance Between Your Application and Gateway](#PerfAppGateway-common)
+ [Measuring Performance Between Your Gateway and AWS](#PerfGatewayAWS-common)
+ [Understanding Volume Metrics](#MonitoringVolumes-common)

Storage Gateway provides CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective on how your gateway and volumes are performing\. For detailed information about CloudWatch, see the *[Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.*

## Using Amazon CloudWatch Metrics<a name="UsingCloudWatchConsole-common"></a>

You can get monitoring data for your gateway using either the AWS Management Console or the CloudWatch API\. The console displays a series of graphs based on the raw data from the CloudWatch API\. You can also use the CloudWatch API through one of the [Amazon AWS Software Development Kits \(SDKs\)](http://aws.amazon.com/code) or the [Amazon CloudWatch API](http://aws.amazon.com/developertools/2534) tools\. Depending on your needs, you might prefer to use either the graphs displayed in the console or retrieved from the API\. 

Regardless of which method you choose to use to work with metrics, you must specify the following information: 

+ The metric dimension to work with\. A *dimension* is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Storage Gateway are `GatewayId`, `GatewayName`, and `VolumeId`\. In the CloudWatch console, you can use the `Gateway Metrics` and `Volume Metrics` views to easily select gateway\-specific and volume\-specific dimensions\. For more information about dimensions, see [Dimensions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*>\.

+ The metric name, such as `ReadBytes`\.

The following table summarizes the types of Storage Gateway metric data that you can use\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/GatewayMetrics-common.html)

Working with gateway and volume metrics is similar to working with other service metrics\. You can find a discussion of some of the most common metrics tasks in the CloudWatch documentation listed following: 

+ [Viewing Available Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)

+ [Getting Statistics for a Metric](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_GetStatistics.html)

+ [Creating CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

## Measuring Performance Between Your Application and Gateway<a name="PerfAppGateway-common"></a>

Data throughput, data latency, and operations per second are three measures that you can use to understand how your application storage that is using your gateway is performing\. When you use the correct aggregation statistic, you can use Storage Gateway metrics to measure these values\. 

A *statistic* is an aggregation of a metric over a specified period of time\. When you view the values of a metric in CloudWatch, use the `Average` statistic for data latency \(milliseconds\), use the `Sum` statistic for data throughput \(bytes per second\), and use the `Samples` statistic for input/output operations per second \(IOPS\)\. For more information, see [Statistics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistics) in the *Amazon CloudWatch User Guide*\. 

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

1. Choose **Create Alarm** to start the Create Alarm Wizard\.

1. Choose the **Storage Gateway** dimension, and find the gateway that you want to work with\.

1. Choose the `CloudBytesUploaded` metric\.

1. To define the alarm, define the alarm state when the `CloudBytesUploaded` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudBytesUploaded` metric is greater than 10 MB for 60 minutes\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.<a name="GatewayAlarm3-common"></a>

**To set an upper threshold alarm for reading data from AWS**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm Wizard\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `CloudDownloadLatency` metric\.

1. Define the alarm by defining the alarm state when the `CloudDownloadLatency` metric is greater than or equal to a specified value for a specified time\. For example, you can define an alarm state when the `CloudDownloadLatency` is greater than 60,000 milliseconds for greater than 2 hours\.

1. Configure the actions to take for the alarm state\. For example, you can have an email notification sent to you\.

1. Choose **Create Alarm**\.

## Understanding Volume Metrics<a name="MonitoringVolumes-common"></a>

You can find information following about the Storage Gateway metrics that cover a volume of a gateway\. Each volume of a gateway has a set of metrics associated with it\. Note that some volume\-specific metrics have the same name as certain gateway\-specific metrics\. These metrics represent the same kinds of measurements but are scoped to the volume instead of the gateway\. You must always specify whether you want to work with either a gateway or a volume metric before working with a metric\. Specifically, when working with volume metrics, you must specify the `VolumeId` that identifies the storage volume for which you are interested in viewing metrics\. For more information, see [Using Amazon CloudWatch Metrics](#UsingCloudWatchConsole-common)\.

The following table describes the Storage Gateway metrics that you can use to get information about your storage volumes\. 


| Metric | Description | Cached volumes | Stored volumes | 
| --- | --- | --- | --- | 
| CacheHitPercent |  Percent of application read operations from the volume that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the volume, this metric reports 100 percent\.  Units: Percent  | yes | no | 
| CachePercentDirty |  The volume's contribution to the overall percentage of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that has not been persisted to AWS\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | yes | no | 
| CachePercentUsed |  The volume's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | yes | no | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | yes | yes | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | yes | yes | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | yes | yes | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | yes | yes | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period\.  Units: Bytes  | yes | yes | 