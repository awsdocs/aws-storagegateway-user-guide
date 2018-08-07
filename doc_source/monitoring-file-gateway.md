# Monitoring Your File Share<a name="monitoring-file-gateway"></a>

You can monitor your file share by using Amazon CloudWatch metrics and use Amazon CloudWatch Events to get notified when your file operations are done\. For information about file gateway type metrics, see [Monitoring Your Gateway and Resources](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html)\.

**Topics**
+ [Getting Notification for File Operations](#get-notification)
+ [Understanding File Share Metrics](#monitoring-fileshare)

## Getting Notification for File Operations<a name="get-notification"></a>

AWS Storage Gateway can send a notification through CloudWatch Events when your file operations are done\. 
+ You can get notified when the gateway finishes uploading your files to your file share\. You can use the [NotifyWhenUploaded](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html) API to request a file upload notification\.
+ You can get notified when the gateway finishes refreshing the cache for your S3 bucket\. You can use the [RefreshCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) API to request a cache refresh notification\.

When the file operation your requested is done, AWS Storage Gateway sends you notification through CloudWatch Events\. You can configure CloudWatch Events to send the notification through event targets such as Amazon SNS, Amazon SQS or AWS Lambda function\. For example, you can configure an Amazon SNS target, to send the notification Amazon SNS consumers such as email and text message\. For information about CloudWatch Events, see [What is Amazon CloudWatch Events?](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html)

**To set up CloudWatch Events notification**

1. Create a target such as an Amazon SNS topic or Lambda function to invoke when the event you requested in AWS Storage Gateway is triggered\.

1. Create a rule in the Amazon CloudWatch Events Console to invoke targets based on an event in AWS Storage Gateway\.

1. In the rule, create an event patten for the event type\. The notification is triggered when the event matches this rule pattern\.

1. Select the target and configure the settings\.

The following example shows a rule that triggers the specified event type in the specified gateway and in the specified AWS Region\. For example, you could specify the `Storage Gateway File Upload Event` as the event type\.

```
{
   "source":[
      "aws.storagegateway"
   ],
   "resources":[
      "arn:aws:storagegateway:AWS Region:account-id
                 :gateway/gateway-id"
   ],
   "detail-type":[
      "Event type"
   ]
}
```

For information about how to create a CloudWatch Events see [Getting Started with Amazon CloudWatch Events](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html)\.

### Getting File Upload Notification<a name="get-upload-notification"></a>

For file notification use case, you could have two file gateways that mapped to the same Amazon S3 bucket and the NFS client for Gateway1 uploads new files to S3\. The files will upload to S3 but they will not appear in Gateway2 because it uses a locally cached version of files in S3\. To make the files visible in gateway2, you can use the [NotifyWhenUploaded](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html) API to request file upload notification from Gateway1 to notify you when the upload is done\. You can then use the CloudWatch Events to automatically issue [RefreshCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) request for the file share on Gateway2\. When the [RefreshCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) request completes the new files will be visible in Gateway2\.

**Example Example—File Upload Notification**  
The following example shows a file upload notification that is sent to you through when the event matches the rule you created\. This notification is in JSON format\. You can configure this notification to be delivered to the target as a text message\.  

```
{
    "id" : "2649b160-d59d-c97f-3f64-8aaa9ea6aed3",
    "version" : "0",
    "account" : "209870788375",
    "source" : "aws.storagegateway",
    "resources" : [
       "arn:aws:storagegateway:us-east-1:123456789011:share/share-F123D451",
       "arn:aws:storagegateway:us-east-1:346332347513:gateway/sgw-712345DA",
       "arn:aws:s3:::mybucket-sgw-aabbcc"
    ],
    "detail" : {
       "event-type" : "upload-complete",
       "notification-id" : "da8db69f-6351-4205-829b-4e82607a00fe",
       "completed" : "2017-11-06T21:34:53Z",
       "request-received" : "2017-11-06T21:34:42Z"
    },
    "detail-type" : "Storage Gateway File Upload Event",
    "region" : "us-east-1",
   "time" : "2017-11-06T21:34:42Z"
}
```

### Getting Refresh Cache Notification<a name="get-refresh-cache-notification"></a>

For refresh cache notification use case, you could have two file gateways that map to the same Amazon S3 bucket and the NFS client for Gateway1 uploads new files to the S3 bucket\. The files will upload to S3 but they will not appear in Gateway2 until you refresh the cache\. This is because Gateway2 uses a locally cached version of the files in S3\. You might want to do something with the files in Gateway2 when the refresh cache is done\. Large files could take a while to show up in gateway2 so you might want to be notified when the cache refresh is done\. You can request refresh cache notification from Gateway2 to notify you when all the files are visible in Gateway2\.

For information about how to create a CloudWatch Events see [Getting Started with Amazon CloudWatch Events](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html)\.

**Example Example—Refresh Cache Notification**  
The following example shows a refresh cache notification that is sent to you through when the event matches the rule you created\. This notification is in JSON format\. You can configure this notification to be delivered to the target as a text message\.  

```
{
    "version" : "0",
    "id" : "2649b160-d59d-c97f-3f64-8aaa9ea6aed3",
    "detail-type" : "Storage Gateway Refresh Cache Event",
    "source" : "aws.storagegateway",
    "account" : "209870788375",
    "time" : "2017-11-06T21:34:42Z",
    "region" : "us-east-2",
    "resources" : [
        "arn:aws:storagegateway:us-east-2:123456789011:share/share-F123D451",
        "arn:aws:storagegateway:us-east-2:346332347513:gateway/sgw-712345DA"
    ],
    "detail" : {
    "event-type" :"refresh-complete",
    "started" : "2018-02-06T21:34:42Z",
    "completed" : "2018-02-06T21:34:53Z"
    }
}
```

## Understanding File Share Metrics<a name="monitoring-fileshare"></a>

You can find information following about the Storage Gateway metrics that cover file shares\. Each file share has a set of metrics associated with it\. Some file share\-specific metrics have the same name as certain gateway\-specific metrics\. These metrics represent the same kinds of measurements but are scoped to the file share instead\. Always specify whether you want to work with either a gateway or a file share metric before working with a metric\. Specifically, when working with file share metrics, you must specify the `File share ID` that identifies the file share for which you are interested in viewing metrics\. For more information, see [Using Amazon CloudWatch Metrics](GatewayMetrics-common.md#UsingCloudWatchConsole-common)\.

The following table describes the Storage Gateway metrics that you can use to get information about your file shares\. 


| Metric | Description | 
| --- | --- | 
| CacheHitPercent |  Percent of application read operations from the file shares that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the file share, this metric reports 100 percent\.  Units: Percent  | 
| CachePercentDirty |  The file share's contribution to the overall percentage of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that has not been persisted to AWS\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | 
| CachePercentUsed |  The file share's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. For more information, see [Understanding Gateway Metrics](Main_monitoring-gateways-common.md#MonitoringGateways-common)\. Units: Percent  | 
| CloudBytesUploaded |  The total number of bytes that the gateway uploaded to AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\.  Units: Bytes  | 
| CloudBytesDownloaded |  The total number of bytes that the gateway downloaded from AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure input/output operations per second \(IOPS\)\. Units: Bytes  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for a file share\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 