# Managing local disks for your Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. Gateways created on Amazon EC2 instances use Amazon EBS volumes as local disks\. 

**Topics**
+ [Deciding the amount of local disk storage](#decide-local-disks-and-sizes)
+ [Determining the size of cache storage to allocate](#CachedLocalDiskCacheSizing-common)
+ [Adding cache storage](#ConfiguringLocalDiskStorage)
+ [Using ephemeral storage with EC2 gateways](#ephemeral-disk-cache)

## Deciding the amount of local disk storage<a name="decide-local-disks-and-sizes"></a>

The number and size of disks that you want to allocate for your gateway is up to you\. The gateway requires the following additional storage:

File gateways require at least one disk to use as a cache\. The following table recommends sizes for local disk storage for your deployed gateway\. You can add more local storage later after you set up the gateway, and as your workload demands increase\.


| Local storage | Description | Gateway type | 
| --- | --- | --- | 
| Cache storage | The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 or file system\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ManagingLocalStorage-common.html)  | 

**Note**  
Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.  
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage\. A data store that is backed by only one underlying physical disk can lead to poor performance in some situations when it is used to back both the cache storage\. This is also true if the backup is a less\-performant RAID configuration such as RAID1\.

After the initial configuration and deployment of your gateway, you can adjust the local storage by adding disks for cache storage\.

## Determining the size of cache storage to allocate<a name="CachedLocalDiskCacheSizing-common"></a>

Your gateway uses its cache storage to provide low\-latency access to your recently accessed data\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 or file system\. For more information about how to estimate your cache storage size, see [Managing local disks for your Storage Gateway](#ManagingLocalStorage-common)\.

You can initially use this approximation to provision disks for the cache storage\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Performance](Performance.md)\.

## Adding cache storage<a name="ConfiguringLocalDiskStorage"></a>

As your application needs change, you can increase the gateway's cache storage capacity\. You can add more cache capacity to your gateway without interrupting existing gateway functions\. When you add more storage capacity, you do so with the gateway VM turned on\.

**Important**  
When adding cache to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache\. Do not remove cache disks that have been allocated as cache storage\.

 The following procedure shows you how to configure or cache storage for your gateway\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To add and configure or cache storage**

1. Provision a new disk in your host \(hypervisor or Amazon EC2 instance\)\. For information about how to provision a disk in a hypervisor, see your hypervisor's user manual\. You configure this disk as cache storage\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. In the **Actions** menu, choose **Edit local disks**\.

1. In the Edit local disks dialog box, identify the disks you provisioned and decide which one you want to use for cached storage\.

   If you don't see your disks, choose the **Refresh** button\.

1. Choose **Save** to save your configuration settings\.

## Using ephemeral storage with EC2 gateways<a name="ephemeral-disk-cache"></a>

This section describes steps you need to take to prevent data loss when you select an ephemeral disk as storage for your gateway's cache\.

Ephemeral disks provide temporary block\-level storage for your Amazon EC2 instance\. Ephemeral disks are ideal for temporary storage of data that changes frequently, such as data in a gateway's cache storage\. When you launch your gateway with an Amazon EC2 Amazon Machine Image, and the instance type you select supports ephemeral storage, the disks are listed automatically and you can select one of the disks to store data in your gateway's cache\. For more information, see [Amazon EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Application writes to the disks are stored in the cache synchronously, and asynchronously uploaded to durable storage in Amazon S3\. If the data stored in the ephemeral storage is lost because an Amazon EC2 instance stopped before data upload was completed, the data that is still in the cache and has not been uploaded to Amazon S3 can be lost\. You can prevent such data loss by following the steps before you restart or stop the EC2 instance that hosts your gateway\.

**Note**  
If you are using ephemeral storage and you stop and start your gateway, the gateway will be permanently offline\. This happens because the physical storage disk is replaced\. There is no work around for this issue so you'd have to delete the gateway and activate a new one on a new EC2 instance\.

These steps in this following procedure are specific for file gateways\.

**To prevent data loss in file gateways that use ephemeral disks**

1. Stop all the processes that are writing to the file share\.

1. Subscribe to receive notification from CloudWatch Events\. For information, see [Getting notified about file operations](monitoring-file-gateway.md#get-notification)\.

1. Call the [NotifyWhenUploaded API](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html) to get notified when data that is written, up until the ephemeral storage was lost, has been durably stored in Amazon S3\.

1. Wait for the API to complete and you receive a notification id\.

   You receive a CloudWatch event with the same notification id\.

1. Verify that the `CachePercentDirty` metric for your file share is 0\. This confirms that all your data has been written to Amazon S3\. For information about file share metrics, see [Understanding file share metrics](monitoring-file-gateway.md#monitoring-file-gateway-resources)\.

1. You can now restart or stop the file gateway without risk of losing any data\.