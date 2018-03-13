# Monitoring Your Gateway and Resources<a name="Main_monitoring-gateways-common"></a>

In this section, you can find information about how to monitor a gateway, including monitoring resources associated with the gateway and monitoring the upload buffer and cache storage\. You use the AWS Management Console to view metrics for your gateway\. For example, you can view the number of bytes used in read and write operations, the time spent in read and write operations, and the time taken to retrieve data from the AWS cloud\. With metrics, you can track the health of your gateway and set up alarms to notify you when one or more metrics fall outside a defined threshold\. 


+ [Understanding Gateway Metrics](#MonitoringGateways-common)
+ [Monitoring the Upload Buffer](#PerfUploadBuffer-common)
+ [Monitoring Cache Storage](#PerfCache-common)
+ [Monitoring Your File Share](monitoring-file-gateway.md)
+ [Monitoring Your Volume Gateway](GatewayMetrics-common.md)
+ [Monitoring Your Tape Gateway](GatewayMetrics-vtl-common.md)
+ [Logging AWS Storage Gateway API Calls by Using AWS CloudTrail](logging-using-cloudtrail-common.md)

AWS Storage Gateway provides Amazon CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective on how your gateway and volumes are performing\. For detailed information about CloudWatch, see the *[Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.

## Understanding Gateway Metrics<a name="MonitoringGateways-common"></a>

For the discussion in this topic, we define *gateway* metrics as metrics that are scoped to the gateway—that is, they measure something about the gateway\. Because a gateway contains one or more volumes, a gateway\-specific metric is representative of all volumes on the gateway\. For example, the `CloudBytesUploaded` metric is the total number of bytes that the gateway sent to the cloud during the reporting period\. This metric includes the activity of all the volumes on the gateway\.

When working with gateway metric data, you specify the unique identification of the gateway that you are interested in viewing metrics for\. To do this, you specify both the `GatewayId` and the `GatewayName` values\. When you want to work with metric for a gateway, you specify the gateway *dimension* in the metrics namespace, which distinguishes a gateway\-specific metric from a volume\-specific metric\. For more information, see [Using Amazon CloudWatch Metrics](GatewayMetrics-common.md#UsingCloudWatchConsole-common)\. 



The following table describes the Storage Gateway metrics that you can use to get information about your gateway\. The entries in the table are grouped functionally by measure\. 

**Note**  
The reporting period for these metrics is 5 minutes\.


| Metric | Description | Applies To\.\. | 
| --- | --- | --- | 
| CacheHitPercent |  Percent of application reads served from the cache\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CachePercentUsed |  Percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CachePercentDirty |  Percent of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CloudBytesDownloaded |  The total number of compressed bytes that the gateway downloaded from AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure input/output operations per second \(IOPS\)\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| CloudDownloadLatency |  The total number of milliseconds spent reading data from AWS during the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| CloudBytesUploaded |  The total number of compressed bytes that the gateway uploaded to AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\.  Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| UploadBufferFree |  The total amount of unused space in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Cached volumes and Tape\.  | 
| CacheFree |  The total amount of unused space in the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes, and Tape\.  | 
| UploadBufferPercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Percent  |   Cached volumes and Tape\.  | 
| UploadBufferUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Cached volumes and Tape\.  | 
| CacheUsed |  The total number of bytes being used in the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes and Tape\.  | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period for all volumes in the gateway\. These bytes are kept in your gateway's working storage\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| TotalCacheSize |  The total size of the cache in bytes\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes, and Tape\.  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period for all volumes in the gateway\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| TimeSinceLastRecoveryPoint |  The time since the last available recovery point\. For more information, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\. Units: Seconds  |  Cached volumes and Stored volumes\.  | 
| WorkingStorageFree |  The total amount of unused space in the gateway's working storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Stored volumes only\.  | 
| WorkingStoragePercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Percent  |  Stored volumes only\.  | 
| WorkingStorageUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Stored volumes only\.  | 

## Monitoring the Upload Buffer<a name="PerfUploadBuffer-common"></a>

You can find information following about how to monitor a gateway's upload buffer and how to create an alarm so that you get a notification when the buffer exceeds a specified threshold\. By using this approach, you can proactively add buffer storage to a gateway before it fills completely and your storage application stops backing up to AWS\.

You monitor the upload buffer in the same way in both the cached volume and tape gateway architectures\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 

**Note**  
The `WorkingStoragePercentUsed`, `WorkingStorageUsed`, and `WorkingStorageFree` metrics represent the upload buffer for the stored volumes setup only before the release of the cached\-volume feature in Storage Gateway\. Now you should use the equivalent upload buffer metrics `UploadBufferPercentUsed`, `UploadBufferUsed`, and `UploadBufferFree`\. These metrics apply to both gateway architectures\.


| Item of Interest | How to Measure | 
| --- | --- | 
| Upload buffer usage |  Use the `UploadBufferPercentUsed`, `UploadBufferUsed`, and `UploadBufferFree` metrics with the `Average` statistic\. For example, use the `UploadBufferUsed` with the `Average` statistic to analyze the storage usage over a time period\.   | <a name="PerfUploadBufferMeasuring-common"></a>

**To measure upload buffer percent used**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `UploadBufferPercentUsed` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percent used of the upload buffer\.

Using the following procedure, you can create an alarm using the CloudWatch console\. To learn more about alarms and thresholds, see [Creating CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\.<a name="GatewayAlarm1-common"></a>

**To set an upper threshold alarm for a gateway's upload buffer**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm Wizard\.

1. Specify a metric for your alarm\.

   1. On the **Select Metric** page of the Create Alarm Wizard, choose the **AWS/StorageGateway:GatewayId,GatewayName** dimension, and then find the gateway that you want to work with\.

   1. Choose the `UploadBufferPercentUsed` metric\. Use the `Average` statistic and a period of 5 minutes\.

   1. Choose **Continue**\.

1. Define the alarm name, description, and threshold\.

   1. On the **Define Alarm** page of the Create Alarm Wizard, identify your alarm by giving it a name and description in the **Name** and **Description** boxes\.

   1. Define the alarm threshold\.

   1. Choose **Continue**\.

1. Configure an email action for the alarm\. 

   1. In the **Configure Actions** page of the Create Alarm Wizard, choose **Alarm** for **Alarm State**\. 

   1. Choose **Choose or create email topic** for **Topic**\.

      To create an email topic means that you set up an Amazon Simple Notification Service \(Amazon SNS\) topic\. For more information about Amazon SNS, see [Set Up Amazon SNS](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html)\.

   1. For **Topic**, type a descriptive name for the topic\.

   1. Choose **Add Action**\.

   1. Choose **Continue**\.

1. Review the alarm settings, and then create the alarm\.

   1. In the **Review** page of the Create Alarm Wizard, review the alarm definition, metric, and associated actions from this step\. Associated actions include, for example, sending an email notification\.

   1. After reviewing the alarm summary, choose **Save Alarm**\.

1. Confirm your subscription to the alarm topic\.

   1. Open the Amazon Simple Notification Service \(Amazon SNS\) email topic that is sent to the email address that you specified when creating the topic\.

      The following image shows a notification\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_76.png)

   1. Confirm your subscription by clicking the link in the email\.

      A subscription confirmation appears\.

## Monitoring Cache Storage<a name="PerfCache-common"></a>

You can find information following about how to monitor a gateway's cache storage and how to create an alarm so that you get a notification when parameters of the cache pass specified thresholds\. Using this alarm, you know when to proactively add cache storage to a gateway\.

You only monitor cache storage in the cached volumes architecture\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 


| Item of Interest | How to Measure | 
| --- | --- | 
| Total usage of cache |  Use the `CachePercentUsed` and `TotalCacheSize` metrics with the `Average` statistic\. For example, use the `CachePercentUsed` with the `Average` statistic to analyze the cache usage over a period of time\.  The `TotalCacheSize` metric changes only when you add cache to the gateway\.  | 
| Percentage of read requests that are served from the cache |  Use the `CacheHitPercent` metric with the `Average` statistic\.  Typically, you want `CacheHitPercent` to remain high\.  | 
| Percentage of cache that is dirty—that is, it contains content that has not been uploaded to AWS |  Use the `CachePercentDirty` metrics with the `Average` statistic\.  Typically, you want `CachePercentDirty` to remain low\.  | <a name="PerfCacheDirtyMeasuring-common1"></a>

**To measure the cache's percentage dirty for a gateway and all its volumes**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `CachePercentDirty` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percentage of the cache that is dirty over the 5 minutes\.<a name="PerfCacheDirtyMeasuring-common"></a>

**To measure the cache's percentage dirty for a volume**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Volume Metrics** dimension, and find the volume that you want to work with\.

1. Choose the `CachePercentDirty` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percentage of the cache that is dirty over the 5 minutes\.