# Monitoring Storage Gateway<a name="Main_monitoring-gateways-common"></a>

In this section, you can find information about how to monitor a gateway, including monitoring resources associated with the gateway and monitoring the upload buffer and cache storage\. You use the AWS Management Console to view metrics for your gateway\. For example, you can view the number of bytes used in read and write operations, the time spent in read and write operations, and the time taken to retrieve data from the AWS cloud\. With metrics, you can track the health of your gateway and set up alarms to notify you when one or more metrics fall outside a defined threshold\. 

**Topics**
+ [Understanding Gateway Metrics](#MonitoringGateways-common)
+ [Dimensions for AWS Storage Gateway Metrics](#storagegateway-metric-dimensions)
+ [Monitoring the Upload Buffer](#PerfUploadBuffer-common)
+ [Monitoring Cache Storage](#PerfCache-common)
+ [Monitoring Your File Gateway](monitoring-file-gateway.md)
+ [Monitoring Your Volume Gateway](monitoring-volume-gateway.md)
+ [Monitoring Your Tape Gateway](GatewayMetrics-vtl-common.md)

AWS Storage Gateway provides Amazon CloudWatch metrics at no additional charge\. Storage Gateway metrics are recorded for a period of two weeks\. By using these metrics, you can access historical information and get a better perspective on how your gateway and volumes are performing\. For detailed information about CloudWatch, see the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.

## Understanding Gateway Metrics<a name="MonitoringGateways-common"></a>

For the discussion in this topic, we define *gateway* metrics as metrics that are scoped to the gateway—that is, they measure something about the gateway\. Because a gateway contains one or more volumes, a gateway\-specific metric is representative of all volumes on the gateway\. For example, the `CloudBytesUploaded` metric is the total number of bytes that the gateway sent to the cloud during the reporting period\. This metric includes the activity of all the volumes on the gateway\.

When working with gateway metric data, you specify the unique identification of the gateway that you are interested in viewing metrics for\. To do this, you specify both the `GatewayId` and the `GatewayName` values\. When you want to work with metric for a gateway, you specify the gateway *dimension* in the metrics namespace, which distinguishes a gateway\-specific metric from a volume\-specific metric\. For more information, see [Using Amazon CloudWatch Metrics](monitoring-volume-gateway.md#UsingCloudWatchConsole-common)\. 

**Topics**


| Metric | Description | Applies To | 
| --- | --- | --- | 
| CacheHitPercent |  Percent of application reads served from the cache\. The sample is taken at the end of the reporting period\. Unit: Percent  |  File, cached\-volume, and tape gateways\.  | 
| UploadBufferPercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Percent  | Cached\-volume and tape gateways\. | 
| UploadBufferUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  Cached\-volume and tape gateways\.  | 
| CacheUsed |  The total number of bytes being used in the gateway's cache storage\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  File, cached\-volume, and tape gateways\.  | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period for all volumes in the gateway\. These bytes are kept in your gateway's working storage\. Unit: Bytes  |  File, cached\-volume, stored\-volume, and tape gateways\.  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Unit: Bytes  |  File, cached\-volume, and stored\-volume gateways\.  | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Average` statistic to measure latency\. Unit: Milliseconds  |  File, cached\-volume, and stored\-volume gateways\.  | 
| TimeSinceLastRecoveryPoint |  The time since the last available recovery point\. For more information, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\. Unit: Seconds  |  Cached volumes and stored volumes\.  | 
| WorkingStorageFree |  The total amount of unused space in the gateway's working storage\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  Stored volumes only\.  | 
| WorkingStoragePercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Percent  |  Stored volumes only\.  | 
| WorkingStorageUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  Stored volumes only\.  | 
| UserCpuPercent |  Percent of CPU time spent on gateway processing, averaged across all cores\. Unit: Percent  |  File gateways only\.  | 
| IoWaitPercent  |  Percent of time that the gateway is waiting on a response from the local disk\. Unit: Percent  |  File gateways only\.  | 
| MemTotalBytes |  Amount of RAM provisioned to the gateway VM, in bytes\. Unit: Bytes  |  File gateways only\.  | 
| MemUsedBytes |  Amount of RAM currently in use by the gateway VM, in bytes\. Unit: Bytes  |  File gateways only\.  | 
| TotalCacheSize |  The total size of the cache in bytes\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  File, cached\-volume, and tape gateways\.  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Unit: Bytes  |  File, cached\-volume, and stored\-volume gateways\.  | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period for all volumes in the gateway\.  Use this metric with the `Average` statistic to measure latency\. Unit: Milliseconds  |  File, cached\-volume, and stored\-volume gateways\.  | 
| TimeSinceLastRecoveryPoint |  The time since the last available recovery point\. For more information, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\. Unit: Seconds  |  Cached volumes and stored volumes\.  | 
| WorkingStorageFree |  The total amount of unused space in the gateway's working storage\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  Stored volumes only\.  | 
| WorkingStoragePercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Percent  |  Stored volumes only\.  | 
| WorkingStorageUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Unit: Bytes  |  Stored volumes only\.  | 
| UserCpuPercent |  Percent of CPU time spent on gateway processing, averaged across all cores\. Unit: Percent  |  File, cached\-volume, stored\-volume, and tape gateways\.  | 
| IoWaitPercent  |  Percent of time that the gateway is waiting on a response from the local disk\. Unit: Percent  |  File, cached\-volume, stored\-volume, and tape gateways\.  | 
| MemTotalBytes |  Amount of RAM provisioned to the gateway VM, in bytes\. Unit: Bytes  |  File, cached\-volume, stored\-volume, and tape gateways\.  | 
| MemUsedBytes |  Amount of RAM currently in use by the gateway VM, in bytes\. Unit: Bytes  |  File, cached\-volume, stored\-volume, and tape gateways\.  | 
| SmbV1Sessions |  The number of Server Message Block \(SMB\) version 1 sessions that are active on the gateway\. Unit: Number  |  File gateways only\.  | 
| SmbV2Sessions |  The number of SMB version 2 sessions that are active on the gateway\. Unit: Number  |  File gateways only\.  | 
| SmbV3Sessions |  The number of SMB version 3 sessions that are active on the gateway\. Unit: Number  |  File gateways only\.  | 
| IndexEvictions |  The number of files whose metadata was evicted from the cached index of file metadata to make room for new entries\. The gateway maintains this metadata index, which is populated from the AWS Cloud on demand\.   Statistics: `SampleCount` for number of directories, `SUM` for total number of files, `Average` for average number of files per directory\. Unit: Number  |  File gateways only\.  | 
| IndexFetches | The number of files for which metadata was fetched\. The gateway maintains a cached index of file metadata, which is populated from the AWS Cloud on demand\. Statistics: `SampleCount` for number of directories, `SUM` for total number of files, `Average` for average number of files per directory\.Unit: Number |  File gateways only\.  | 
| AvailabilityNotifications | Number of availability\-related health notifications generated by the gateway\.Use this metric with the `Sum` statistic to observe whether the gateway is experiencing any availability\-related events\. For details about the events, check your configured CloudWatch log group\.Unit: Number |  All gateways\.  | 

## Dimensions for AWS Storage Gateway Metrics<a name="storagegateway-metric-dimensions"></a>

The Amazon CloudWatch namespace for the AWS Storage Gateway service is `AWS/StorageGateway`\. Data is available automatically in 5\-minute periods at no charge\. 


|  Dimension  |  Description  | 
| --- | --- | 
|  GatewayId, GatewayName |  These dimensions filter the data that you request to gateway\-specific metrics\. You can identify a gateway to work by the value for `GatewayId` or `GatewayName`\. If the name of your gateway was different for the time range that you are interested in viewing metrics, use the `GatewayId`\.   Throughput and latency data of a gateway is based on all the volumes for the gateway\. For information about working with gateway metrics, see [Measuring Performance Between Your Gateway and AWS](https://docs.aws.amazon.com/storagegateway/latest/userguide/monitoring-volume-gateway.html#PerfGatewayAWS-common)\.   | 
|  VolumeId  |  This dimension filters the data you request to volume\-specific metrics\. Identify a storage volume to work with by its `VolumeId` value\. For information about working with volume metrics, see [Measuring Performance Between Your Application and Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/monitoring-volume-gateway.html#PerfAppGateway-common)\.   | 

## Monitoring the Upload Buffer<a name="PerfUploadBuffer-common"></a>

You can find information following about how to monitor a gateway's upload buffer and how to create an alarm so that you get a notification when the buffer exceeds a specified threshold\. By using this approach, you can add buffer storage to a gateway before it fills completely and your storage application stops backing up to AWS\.

You monitor the upload buffer in the same way in both the cached\-volume and tape gateway architectures\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 

**Note**  
The `WorkingStoragePercentUsed`, `WorkingStorageUsed`, and `WorkingStorageFree` metrics represent the upload buffer for stored volumes only before the release of the cached\-volume feature in Storage Gateway\. Now, use the equivalent upload buffer metrics `UploadBufferPercentUsed`, `UploadBufferUsed`, and `UploadBufferFree`\. These metrics apply to both gateway architectures\.


| Item of Interest | How to Measure | 
| --- | --- | 
| Upload buffer usage |  Use the `UploadBufferPercentUsed`, `UploadBufferUsed`, and `UploadBufferFree` metrics with the `Average` statistic\. For example, use the `UploadBufferUsed` with the `Average` statistic to analyze the storage usage over a time period\.   | <a name="PerfUploadBufferMeasuring-common"></a>

**To measure the percent of the upload buffer that is used**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `UploadBufferPercentUsed` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percent used of the upload buffer\.

Using the following procedure, you can create an alarm using the CloudWatch console\. To learn more about alarms and thresholds, see [Creating CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.<a name="GatewayAlarm1-common"></a>

**To set an upper threshold alarm for a gateway's upload buffer**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Create Alarm** to start the Create Alarm wizard\.

1. Specify a metric for your alarm:

   1. On the **Select Metric** page of the Create Alarm wizard, choose the **AWS/StorageGateway:GatewayId,GatewayName** dimension, and then find the gateway that you want to work with\.

   1. Choose the `UploadBufferPercentUsed` metric\. Use the `Average` statistic and a period of 5 minutes\.

   1. Choose **Continue**\.

1. Define the alarm name, description, and threshold:

   1. On the **Define Alarm** page of the Create Alarm wizard, identify your alarm by giving it a name and description in the **Name** and **Description** boxes\.

   1. Define the alarm threshold\.

   1. Choose **Continue**\.

1. Configure an email action for the alarm: 

   1. On the **Configure Actions** page of the Create Alarm wizard, choose **Alarm** for **Alarm State**\. 

   1. Choose **Choose or create email topic** for **Topic**\.

      To create an email topic means that you set up an Amazon SNS topic\. For more information about Amazon SNS, see [Set Up Amazon SNS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) in *Amazon CloudWatch User Guide\.*

   1. For **Topic**, enter a descriptive name for the topic\.

   1. Choose **Add Action**\.

   1. Choose **Continue**\.

1. Review the alarm settings, and then create the alarm:

   1. On the **Review** page of the Create Alarm wizard, review the alarm definition, metric, and associated actions to take \(for example, sending an email notification\)\.

   1. After reviewing the alarm summary, choose **Save Alarm**\.

1. Confirm your subscription to the alarm topic:

   1. Open the Amazon SNS email that was sent to the email address that you specified when creating the topic\.

      The following image shows a typical email notification\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMetrics_76.png)

   1. Confirm your subscription by clicking the link in the email\.

      A subscription confirmation appears\.

## Monitoring Cache Storage<a name="PerfCache-common"></a>

You can find information following about how to monitor a gateway's cache storage and how to create an alarm so that you get a notification when parameters of the cache pass specified thresholds\. Using this alarm, you know when to add cache storage to a gateway\.

You only monitor cache storage in the cached volumes architecture\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 


| Item of Interest | How to Measure | 
| --- | --- | 
| Total usage of cache |  Use the `CachePercentUsed` and `TotalCacheSize` metrics with the `Average` statistic\. For example, use the `CachePercentUsed` with the `Average` statistic to analyze the cache usage over a period of time\.  The `TotalCacheSize` metric changes only when you add cache to the gateway\.  | 
| Percent of read requests that are served from the cache |  Use the `CacheHitPercent` metric with the `Average` statistic\.  Typically, you want `CacheHitPercent` to remain high\.  | 
| Percent of the cache that is dirty—that is, it contains content that has not been uploaded to AWS |  Use the `CachePercentDirty` metrics with the `Average` statistic\.  Typically, you want `CachePercentDirty` to remain low\.  | <a name="PerfCacheDirtyMeasuring-common1"></a>

**To measure the percent of a cache that is dirty for a gateway and all its volumes**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Gateway Metrics** dimension, and find the gateway that you want to work with\.

1. Choose the `CachePercentDirty` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percentage of the cache that is dirty over the 5 minutes\.<a name="PerfCacheDirtyMeasuring-common"></a>

**To measure the percent of the cache that is dirty for a volume**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose the **StorageGateway: Volume Metrics** dimension, and find the volume that you want to work with\.

1. Choose the `CachePercentDirty` metric\.

1. For **Time Range**, choose a value\.

1. Choose the `Average` statistic\.

1. For **Period**, choose a value of 5 minutes to match the default reporting time\. 

The resulting time\-ordered set of data points contains the percentage of the cache that is dirty over the 5 minutes\.