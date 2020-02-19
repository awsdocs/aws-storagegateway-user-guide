# Managing Local Disks for Your AWS Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. Gateways created on Amazon EC2 instances use Amazon EBS volumes as local disks\. 

**Topics**
+ [Deciding the Amount of Local Disk Storage](#decide-local-disks-and-sizes)
+ [Determining the Size of Upload Buffer to Allocate](#CachedLocalDiskUploadBufferSizing-common)
+ [Determining the Size of Cache Storage to Allocate](#CachedLocalDiskCacheSizing-common)
+ [Adding an Upload Buffer or Cache Storage](#ConfiguringLocalDiskStorage)
+ [Using Ephemeral Storage With EC2 Gateways](#ephemeral-disk-cache)

## Deciding the Amount of Local Disk Storage<a name="decide-local-disks-and-sizes"></a>

The number and size of disks that you want to allocate for your gateway is up to you\. Depending on the storage solution you deploy \(see [Plan Your Storage Gateway Deployment](WhatIsStorageGateway.md#planning-gateway-deployment)\), the gateway requires the following additional storage:
+ File gateways require at least one disk to use as a cache\.
+ Volume gateways:
  + Stored gateways require at least one disk to use as an upload buffer\.
  + Cached gateways require at least two disks\. One to use as a cache, and one to use as an upload buffer\.
+ Tape gateways require at least two disks\. One to use as a cache, and one to use as an upload buffer\.

The following table recommends sizes for local disk storage for your deployed gateway\. You can add more local storage later after you set up the gateway, and as your workload demands increase\.


| Local Storage | Description | Gateway Type | 
| --- | --- | --- | 
| Upload buffer | The upload buffer provides a staging area for the data before the gateway uploads the data to Amazon S3\. Your gateway uploads this buffer data over an encrypted Secure Sockets Layer \(SSL\) connection to AWS\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ManagingLocalStorage-common.html)  | 
| Cache storage | The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. When your application performs I/O on a volume or tape, the gateway saves the data to the cache storage for low\-latency access\. When your application requests data from a volume or tape, the gateway first checks the cache storage for the data before downloading the data from AWS\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ManagingLocalStorage-common.html)  | 

**Note**  
When you provision disks, we strongly recommend that you do not provision local disks for the upload buffer and cache storage if they use the same physical resource \(the same disk\)\. Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage or upload buffer\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.   
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage and another for the upload buffer\. A data store that is backed by only one underlying physical disk can lead to poor performance in some situations when it is used to back both the cache storage and upload buffer\. This is also true if the backup is a less\-performant RAID configuration such as RAID1\.

After the initial configuration and deployment of your gateway, you can adjust the local storage by adding or removing disks for an upload buffer\. You can also add disks for cache storage\. 

## Determining the Size of Upload Buffer to Allocate<a name="CachedLocalDiskUploadBufferSizing-common"></a>

You can determine the size of your upload buffer to allocate by using an upload buffer formula\. We strongly recommend that you allocate at least 150 GiB of upload buffer\. If the formula returns a value less than 150 GiB, use 150 GiB as the amount you allocate to the upload buffer\. You can configure up to 2 TiB of upload buffer capacity for each gateway\. 

**Note**  
For volume gateways, when the upload buffer reaches its capacity, your volume goes to PASS THROUGH status\. In this status, new data that your application writes is persisted locally but not uploaded to AWS immediately\. Thus, you cannot take new snapshots\. When the upload buffer capacity frees up, the volume goes through BOOTSTRAPPING status\. In this status, any new data that was persisted locally is uploaded to AWS\. Finally, the volume returns to ACTIVE status\. Storage Gateway then resumes normal synchronization of the data stored locally with the copy stored in AWS, and you can start taking new snapshots\. For more information about volume status, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\.  
For tape gateways, when the upload buffer reaches its capacity, your applications can continue to read from and write data to your storage volumes\. However, the tape gateway does not write any of your volume data to its upload buffer and does not upload any of this data to AWS until Storage Gateway synchronizes the data stored locally with the copy of the data stored in AWS\. This synchronization occurs when the volumes are in BOOTSTRAPPING status\.

To estimate the amount of upload buffer to allocate, you can determine the expected incoming and outgoing data rates and plug them into the following formula\.

**Rate of incoming data**  
This rate refers to the application throughput, the rate at which your on\-premises applications write data to your gateway over some period of time\.

**Rate of outgoing data**  
This rate refers to the network throughput, the rate at which your gateway is able to upload data to AWS\. This rate depends on your network speed, utilization, and whether you've enabled bandwidth throttling\. This rate should be adjusted for compression\. When uploading data to AWS, the gateway applies data compression where possible\. For example, if your application data is text\-only, you might get an effective compression ratio of about 2:1\. However, if you are writing videos, the gateway might not be able to achieve any data compression and might require more upload buffer for the gateway\.

We strongly recommend that you allocate at least 150 GiB of upload buffer space if either of the following is true:
+ Your incoming rate is higher than the outgoing rate\.
+ The formula returns a value less than 150 GiB\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/WorkingStorageFormula-diagram.png)

For example, assume that your business applications write text data to your gateway at a rate of 40 MB per second for 12 hours per day and your network throughput is 12 MB per second\. Assuming a compression factor of 2:1 for the text data, you would allocate approximately 690 GiB of space for the upload buffer\.

**Example**  

```
1. ((40 MB/sec) - (12 MB/sec * 2)) * (12 hours * 3600 seconds/hour) = 691200 megabytes
```

You can initially use this approximation to determine the disk size that you want to allocate to the gateway as upload buffer space\. Add more upload buffer space as needed using the Storage Gateway console\. Also, you can use the Amazon CloudWatch operational metrics to monitor upload buffer usage and determine additional storage requirements\. For information on metrics and setting the alarms, see [Monitoring the Upload Buffer](Main_monitoring-gateways-common.md#PerfUploadBuffer-common)\.

## Determining the Size of Cache Storage to Allocate<a name="CachedLocalDiskCacheSizing-common"></a>

Your gateway uses its cache storage to provide low\-latency access to your recently accessed data\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. Generally speaking, you size the cache storage at 1\.1 times the upload buffer size\. For more information about how to estimate your cache storage size, see [Determining the Size of Upload Buffer to Allocate](#CachedLocalDiskUploadBufferSizing-common)\.

You can initially use this approximation to provision disks for the cache storage\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Monitoring Cache Storage](Main_monitoring-gateways-common.md#PerfCache-common)\.

## Adding an Upload Buffer or Cache Storage<a name="ConfiguringLocalDiskStorage"></a>

As your application needs change, you can increase the gateway's upload buffer or cache storage capacity\. You can add more buffer capacity to your gateway without interrupting existing gateway functions\. When you add more upload buffer capacity, you do so with the gateway VM turned on\.

**Important**  
When adding cache or upload buffer to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\. Do not remove cache disks that have been allocated as cache storage\. 

 The following procedure shows you how to configure an upload buffer or cache storage for your gateway\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To add and configure upload buffer or cache storage**

1. Provision a new disk in your host \(hypervisor or Amazon EC2 instance\)\. For information about how to provision a disk in a hypervisor, see your hypervisor's user manual\. For information about how to add Amazon EBS volumes for your Amazon EC2 instance, see [Adding and Removing Amazon EBS Volumes for Your Gateway Hosted on Amazon EC2](GatewayInstanceStorage-common.md)\. You configure this disk as an upload buffer or cache storage\. 

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. In the **Actions** menu, choose **Edit local disks**\.

1. In the Edit local disks dialog box, identify the disks you provisioned and decide which one you want to use for an upload buffer or cached storage\. 
**Note**  
For stored volumes, only the upload buffer is displayed because stored volumes have no cache disks\. 

1. In the drop\-down list box, in the **Allocated to** column, choose **Upload Buffer** for the disk to use as an upload buffer\.

1. For gateways created with cached volumes and tape gateway, choose **Cache** for the disk you want to use as a cache storage\.

   If you don't see your disks, choose the **Refresh** button\.

1. Choose **Save** to save your configuration settings\.

## Using Ephemeral Storage With EC2 Gateways<a name="ephemeral-disk-cache"></a>

This section describes steps you need to take to prevent data loss when you select an ephemeral disk as storage for your gateway's cache\.

Ephemeral disks provide temporary block\-level storage for your Amazon EC2 instance\. Ephemeral disks are ideal for temporary storage of data that changes frequently, such as data in a gateway's upload buffer or cache storage\. When you launch your gateway with an Amazon EC2 Amazon Machine Image, and the instance type you select supports ephemeral storage, the disks are listed automatically and you can select one of the disks to store data in your gateway's cache\. For more information, see [Amazon EC2 Instance Store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Application writes to the disks are stored in the cache synchronously, and asynchronously uploaded to durable storage in Amazon S3\. If the data stored in the ephemeral storage is lost because an Amazon EC2 instance stopped before data upload was completed, the data that is still in the cache and has not been uploaded to Amazon S3 can be lost\. You can prevent such data loss by following the steps before you restart or stop the EC2 instance that hosts your gateway\.

**Note**  
If you are using ephemeral storage and you stop and start your gateway, the gateway will be permanently offline\. This happens because the physical storage disk is replaced\. There is no work around for this issue so you'd have to delete the gateway and activate a new one on a new Amazon EC2 instance\.

These steps in this following procedure are specific for file gateways\.

**To prevent data loss in file gateways that use ephemeral disks**

1. Stop all the processes that are writing to the file share\.

1. Subscribe to receive notification from CloudWatch Events\. For information, see [Getting File Upload Notification](monitoring-file-gateway.md#get-upload-notification)\.

1. Call the [NotifyWhenUploaded API](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html) to get notified when data that is written, up until the ephemeral storage was lost, has been durably stored in Amazon S3\.

1. Wait for the API to complete and you receive a notification id\.

   You receive a CloudWatch event with the same notification id\.

1. Verify that the `CachePercentDirty` metric for your file share is 0\. This confirms that all your data has been written to Amazon S3\. For information about file share metrics, see [Understanding File Share Metrics](monitoring-file-gateway.md#monitoring-fileshare)\.

1. You can now restart or stop the file gateway without risk of losing any data\.