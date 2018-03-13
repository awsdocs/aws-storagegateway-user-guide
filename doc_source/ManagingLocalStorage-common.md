# Managing Local Disks for Your AWS Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. For cached volumes and tape gateways, you allocate two disks, one disk for the upload buffer and the other for cache storage\. For stored volumes, you allocate one disk for the upload buffer\.

**Important**  
When adding cache or upload buffer to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\. Do not remove cache disks that have been allocated as cache storage\.


+ [Deciding the Amount of Local Disk Storage](#decide-local-disks-and-sizes)
+ [Configuring Local Storage for Your Gateway](#configuring-local-storage-common)
+ [Adding and Removing Upload Buffer](#GatewayCachedUploadBuffer)
+ [Adding Cache Storage](#managing-cache-common)

## Deciding the Amount of Local Disk Storage<a name="decide-local-disks-and-sizes"></a>

In this step, you decide the number and size of disks to allocate for your gateway\. Depending on the storage solution you deploy \(see [Plan Your Storage Gateway Deployment](WhatIsStorageGateway.md#planning-gateway-deployment)\), the gateway requires the following additional storage:

+ File gateways require at least one disk to use as a cache\.

+ Volume gateways:

  + Stored gateways require at least one disk to use as an upload buffer\.

  + Cached gateways require at least two disks\. One to use as a cache, and one to use as an upload buffer\.

+ Tape gateways require at least two disks\. One to use as a cache, and one to use as an upload buffer\.

For information about recommended disk sizes, see [Recommended Local Disk Sizes For Your Gateway](resource-gateway-limits.md#disk-sizes)\. If you plan to deploy your gateway in production, you should consider your real workload in determining disk sizes\. For information about disk size guidelines, see [Adding and Removing Upload Buffer](#GatewayCachedUploadBuffer) and [Adding Cache Storage](#managing-cache-common)\.

For more information about how gateways use local storage, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. In the next step, you allocate the local disk storage to the gateway VM you deployed\.

The following table recommends sizes for local disk storage for your deployed gateway\. Before going to the next step, decide the number and size of disks to allocate\. You can add more local storage after you set the gateway up, and as your workload demands\.


| Local Storage | Description | Gateway Type | 
| --- | --- | --- | 
| Upload buffer | The upload buffer provides a staging area for the data before the gateway uploads the data to Amazon S3\. Your gateway uploads this buffer data over an encrypted Secure Sockets Layer \(SSL\) connection to AWS\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ManagingLocalStorage-common.html)  | 
| Cache storage | The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. When your application performs I/O on a volume or tape, the gateway saves the data to the cache storage for low\-latency access\. When your application requests data from a volume or tape, the gateway first checks the cache storage for the data before downloading the data from AWS\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ManagingLocalStorage-common.html)  | 

**Note**  
When you provision disks, we strongly recommend that you do not provision local disks for the upload buffer and cache storage that use the same underlying physical storage resource \(that is, the same disk\)\. Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage or upload buffer\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.   
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage and another for the upload buffer\. A data store that is backed by only one underlying physical disk, or that is backed by a less\-performant RAID configuration such as RAID 1, can lead to poor performance in some situations when used to back both the cache storage and upload buffer\. 

After the initial configuration and deployment of your gateway, you might find that you need to adjust the local storage by adding or removing disks for an upload buffer or adding disks for cache storage\. 

## Configuring Local Storage for Your Gateway<a name="configuring-local-storage-common"></a>

When you created your gateway, you allocated disks for your gateway to use as upload buffer or cache storage\. The upload buffer and cache storage are created from local disks you provisioned for your gateway VM when you first created your gateway\. After your gateway is up and running, you might decide to configure additional upload buffer or cache storage for your gateway\. You use the suggested sizing formula in deciding the disk sizes\. For more information on sizing storage, see [Adding and Removing Upload Buffer](#GatewayCachedUploadBuffer) or [Adding Cache Storage](#managing-cache-common)\. If you are configuring local storage for the first time, see [Configuring Local Disks](create-gateway-vtl.md#configure-local-disk-alarms) for instructions\. 

**Important**  
When adding cache or upload buffer to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\. Do not remove cache disks that have been allocated as cache storage\. 

### Configuring an Upload Buffer or Cache Storage<a name="ConfiguringLocalDiskStorage"></a>

After your gateway is activated, you might need to add additional disks and configure them as local storage\. The following procedure shows you how to configure an upload buffer or cache storage for your gateway\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To configure upload buffer or cache storage**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. In the **Actions** menu, choose **Edit local disks**\.

1. In the Edit local disks dialog box, identify the disks you provisioned and decide which one you want to use for upload buffer or cached storage\. 
**Note**  
For stored volumes, only the upload buffer is displayed because stored volumes have no cache disks\. 

1. In the drop\-down list box, in the **Allocate to** column, choose **Upload Buffer** for the disk to use as upload buffer\.

1. For gateways created with cached volumes and tape gateway, choose **Cache** for the disk you want to use as a cache storage\.

   If you don't see your disks, choose the **Refresh** button\.

1. Choose **Save** to save your configuration settings\.

For stored volumes, you configure one of the two disks for use by your application's data and the other disk as an upload buffer\.

## Adding and Removing Upload Buffer<a name="GatewayCachedUploadBuffer"></a>

After you configure your initial gateway, you can allocate and configure additional upload buffer capacity or reduce the capacity as your application needs change\. To learn more about how to size your upload buffer based on your application needs, see [Sizing the Upload Buffer](#CachedLocalDiskUploadBufferSizing-common)\.


+ [Adding Upload Buffer Capacity](#GatewayCachedUploadBufferAdding)
+ [Removing Upload Buffer Capacity](#GatewayCachedUploadBufferRemoving)
+ [Sizing the Upload Buffer](#CachedLocalDiskUploadBufferSizing-common)

### Adding Upload Buffer Capacity<a name="GatewayCachedUploadBufferAdding"></a>

As your application needs change and you add more volume capacity, you might need to increase the gateway's upload buffer capacity as well\. You can add more buffer capacity to your gateway without interrupting existing gateway functions\. Note that when you add more upload buffer capacity, you do so with the gateway VM turned on\. However, when you reduce the amount of upload buffer capacity, you must first turn off the VM\. You can add more upload buffer capacity by using the Storage Gateway console or the Storage Gateway API: 

+ For information on adding buffer capacity with the console, see [To configure upload buffer or cache storage ](#GatewayWorkingStorageCachedTaskBuffer)\. This procedure assumes that your gateway has at least one local disk available on its VM that you can allocate as an upload buffer to the gateway\.

+ For information on adding buffer capacity with the API, see [AddUploadBuffer](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddUploadBuffer.html)\. 

### Removing Upload Buffer Capacity<a name="GatewayCachedUploadBufferRemoving"></a>

As your application needs change and you change the volume configuration for a gateway, you might need to decrease the gateway's upload buffer capacity\. Or, a local disk allocated as upload buffer space might fail and you might need to remove that disk from your upload buffer and assign a new local disk\. In both cases, you can remove buffer capacity using the Storage Gateway console\. 

The following procedure assumes that your activated gateway has at least one local disk allocated as an upload buffer for the gateway\. In the procedure, you start on the Storage Gateway console, leave the console and use the VMware vSphere client or the Microsoft Hyper\-V Manager to remove the disk, and then return to the console\.  <a name="GatewayCachedUploadBufferRemovingTask"></a>

**To find the ID of a disk allocated as an upload buffer**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. On the **Actions** menu, choose **Edit local Disks**\.

1. In the **Edit local disks** dialog box, note the value of the virtual device node for the local disk to be removed\. You can find the node value in the **Disk ID** column\.

   You use the disk's virtual device node in the vSphere client to help ensure that you remove the correct disk\.

1. Stop the gateway by following the steps in the [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md) procedure\.
**Note**  
Before you stop the gateway, ensure that no application is writing data to it and that no snapshots are in progress\. You can check the snapshot schedule of volumes on the **Snapshot Schedules** tab of the Storage Gateway console\. For more information, see [Editing a Snapshot Schedule](managing-volumes.md#SchedulingSnapshot)\. 

1. To remove the underlying local disk, do one of the following procedures\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ManagingLocalStorage-common.html)

1. On the Storage Gateway console, turn on the gateway\.
**Important**  
After removing a disk used as an upload buffer, you must turn the gateway back on before adding new disks to the VM\. 

   After a gateway restart, a storage volume might go through the [PASS THROUGH](managing-volumes.md#VolumeStatusPASSTHROUGH) and [BOOTSTRAPPING](managing-volumes.md#VolumeStatusBOOTSTRAPPING) states as the gateway adjusts to the upload buffer disk that you removed\. A volume that passes through these two states will eventually come to the ACTIVE state\. You can use a volume during the PASS THROUGH and BOOTSTRAPPING states\. However, you cannot take snapshots of the volume in these states\. You can monitor your volume status in the **Volumes** tab on the Storage Gateway console\.

### Sizing the Upload Buffer<a name="CachedLocalDiskUploadBufferSizing-common"></a>

You can determine the size of your upload buffer by using an upload buffer formula\. We strongly recommend that you allocate at least 150 GiB of upload buffer\. If the formula returns a value less than 150 GiB, use 150 GiB as the amount you allocate to the upload buffer\. You can configure up to 2 TiB of upload buffer capacity for each gateway\. 

**Note**  
For volume gateways, when the upload buffer reaches its capacity, your volume goes to PASS THROUGH status\. In this status, new data that your application writes is persisted locally but not uploaded to AWS immediately\. Thus, you cannot take new snapshots\. When the upload buffer capacity frees up, the volume goes through BOOTSTRAPPING status\. In this status, any new data that was persisted locally is uploaded to AWS\. Finally, the volume returns to ACTIVE status\. Storage Gateway then resumes normal synchronization of the data stored locally with the copy stored in AWS, and you can start taking new snapshots\. For more information about volume status, see [Understanding Volume Status](managing-volumes.md#StorageVolumeStatuses)\.  
For tape gateways, when the upload buffer reaches its capacity, your applications can continue to read from and write data to your storage volumes\. However, the tape gateway does not write any of your volume data to its upload buffer and does not upload any of this data to AWS until Storage Gateway synchronizes the data stored locally with the copy of the data stored in AWS\. This synchronization occurs when the volumes are in BOOTSTRAPPING status\.

To estimate the amount of upload buffer, you can determine the expected incoming and outgoing data rates and plug them into the following formula\.

**Rate of incoming data**  
This rate refers to the application throughput, the rate at which your on\-premises applications write data to your gateway over some period of time\.

**Rate of outgoing data**  
This rate refers to the network throughput, the rate at which your gateway is able to upload data to AWS\. This rate depends on your network speed, utilization, and whether you've enabled bandwidth throttling\. This rate should be adjusted for compression\. When uploading data to AWS, the gateway applies data compression where possible\. For example, if your application data is text\-only, you might get an effective compression ratio of about 2:1\. However, if you are writing videos, the gateway might not be able to achieve any data compression and might require more upload buffer for the gateway\.

If your incoming rate is higher than the outgoing rate, or if the formula returns a value less than 150 GiB, we strongly recommend that you allocate at least 150 GiB of upload buffer space\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/WorkingStorageFormula-diagram.png)

For example, assume that your business applications write text data to your gateway at a rate of 40 MB a second for 12 hours a day and your network throughput is 12 MB a second\. Assuming a compression factor of 2:1 for the text data, you need to allocate approximately 690 GiB of space for the upload buffer\.

**Example**  

```
1. ((40 MB/sec) - (12 MB/sec * 2)) * (12 hours * 3600 seconds/hour) = 691200 megabytes
```

Note that you can initially use this approximation to determine the disk size that you want to allocate to the gateway as upload buffer space\. Add more upload buffer space as needed using the Storage Gateway console\. Also, you can use the Amazon CloudWatch operational metrics to monitor upload buffer usage and determine additional storage requirements\. For information on metrics and setting the alarms, see [Monitoring the Upload Buffer](Main_monitoring-gateways-common.md#PerfUploadBuffer-common)\.

If you decide that you need to change your upload buffer capacity, take one of the following actions\.


| To | Do This | 
| --- | --- | 
| Add more upload buffer capacity to your gateway\. |  Follow the steps in [Adding Upload Buffer Capacity](#GatewayCachedUploadBufferAdding)\.  | 
| Remove a disk allocated as upload buffer space\. |  Follow the steps in [Removing Upload Buffer Capacity](#GatewayCachedUploadBufferRemoving)\.  | 

## Adding Cache Storage<a name="managing-cache-common"></a>

The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\.

**Important**  
Gateways created with stored volumes don't require cache storage\.

**Important**  
When adding cache or upload buffer to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\. Do not remove cache disks that have been allocated as cache storage\.


+ [Sizing Cache Storage](#CachedLocalDiskCacheSizing-common)
+ [Adding Cache Storage for Your Gateway](#GatewayCachedCacheStorage)

The following diagram highlights the cache storage in the larger picture of the cached volumes architecture\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ArchitectureDiagramCached_HighlightedCache-diagram.png)

The following diagram highlights the cache storage in the larger picture of the tape gateway architecture\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Gateway-VTL-ArchitectureCachedStorage-vtl-diagram.png)

The amount of cache storage your gateway requires depends on how much of your application data you want to provide low\-latency access to\. The cache storage must be at least the size of the upload buffer\. This guideline helps ensure that the cache storage is large enough to persistently hold all data that has not yet been uploaded to Amazon S3\. When your cache storage has filled up with dirty data \(that is, data that has not been uploaded to AWS\), application write operations to your volumes or tapes are blocked until more cache storage becomes available\. However, application read operations from the volume or tapes are still allowed\.

Here are some guidelines you can follow to help ensure you have adequate cache storage allocated for your gateway\. 

+ ****Use the sizing formula\.**** – As your application needs change, you should periodically review the recommended formula for sizing cache storage\. For more information, see [Sizing Cache Storage](#CachedLocalDiskCacheSizing-common)\.

+ ****Use Amazon CloudWatch metrics\.**** – You can proactively avoid filling up cache storage with dirty data by monitoring how cache storage is being used—particularly, by reviewing cache misses\. CloudWatch provides usage metrics such as the `CachePercentDirty` and `CacheHitPercent` metrics for monitoring how much of the gateway's cache storage has not been uploaded to Amazon S3\. You can set an alarm to trigger a notification to you when the percentage of the cache that is dirty exceeds a threshold or the cache hit percentage falls below a threshold\. Both of these can indicate that the cache storage size is not adequate for the gateway\. For a full list of Storage Gateway metrics, see [Monitoring Your Gateway and Resources](Main_monitoring-gateways-common.md)\.

### Sizing Cache Storage<a name="CachedLocalDiskCacheSizing-common"></a>

Your gateway uses its cache storage to provide low\-latency access to your recently accessed data\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. Generally speaking, you size the cache storage at 1\.1 times the upload buffer size\. For more information about how to estimate your cache storage size, see [Sizing the Upload Buffer](#CachedLocalDiskUploadBufferSizing-common)\.

You can initially use this approximation to provision disks for the cache storage\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Monitoring Cache Storage](Main_monitoring-gateways-common.md#PerfCache-common)\.

If you decide that you need to increase your gateway's cache storage capacity, follow the steps in [Adding Cache Storage for Your Gateway](#GatewayCachedCacheStorage)\.

### Adding Cache Storage for Your Gateway<a name="GatewayCachedCacheStorage"></a>

After you configure your initial gateway cache storage as described in [Configuring an Upload Buffer or Cache Storage](#ConfiguringLocalDiskStorage), you can add cache storage to your gateway as your application needs change\. To learn more about how to size your cache storage based on your application needs, see [Adding Cache Storage](#managing-cache-common)\.

You can add more cache storage to your gateway without interrupting existing gateway functions and with the gateway VM turned on\. 

You can add more cache storage by using the Storage Gateway console or the Storage Gateway API: 

+ For information on adding cache storage using the console, [To configure upload buffer or cache storage ](#GatewayWorkingStorageCachedTaskBuffer)\. This procedure assumes that your activated gateway has at least one local disk available on its VM that you can allocate as cache storage for the gateway\. Don't remove cache disks that have been allocated as cache storage\.

+ For information on adding cache storage by using the API, see [AddCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddCache.html)\. 