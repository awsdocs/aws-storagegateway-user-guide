# Adding and Removing Amazon EBS Volumes for Your Gateway Hosted on Amazon EC2<a name="GatewayInstanceStorage-common"></a>

When you initially configured your gateway to run as an Amazon EC2 instance, you allocated Amazon EBS volumes for use as an upload buffer and cache storage\. Over time, as your applications needs change, you can allocate additional Amazon EBS volumes for this use\. You can also reduce the storage you allocated by removing previously allocated Amazon EBS volumes\. For more information about Amazon EBS, see [Amazon Elastic Block Store \(Amazon EBS\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Before you add more storage to the gateway, you should review how to size your upload buffer and cache storage based on your application needs for a gateway\. To do so, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common) and [Determining the Size of Cache Storage to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskCacheSizing-common)\.

There are limits to the maximum storage you can allocate as an upload buffer and cache storage\. You can attach as many Amazon EBS volumes to your instance as you want, but you can only configure these volumes as upload buffer and cache storage space up to these storage limits\. For more information, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\.<a name="EC2GatewayAddBlockStorage-common"></a>

**To add an Amazon EBS volume and configure it for your gateway**

1. Create an Amazon EBS volume\. For instructions, see [Creating or Restoring an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Attach the Amazon EBS volume to your Amazon EC2 instance\. For instructions, see [Attaching an Amazon EBS Volume to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Configure the Amazon EBS volume you added as either an upload buffer or cache storage\. For instructions, see [Managing Local Disks for Your AWS Storage Gateway](ManagingLocalStorage-common.md)\.

There are times you might find you don’t need the amount of storage you allocated for the upload buffer\. <a name="EC2GatewayRemoveBlockStorage-common"></a>

**To remove an Amazon EBS volume**
**Warning**  
These steps apply only for Amazon EBS volumes allocated as upload buffer space\. If you remove an Amazon EBS volume that is allocated as cache storage from a gateway, virtual tapes on the gateway will have the IRRECOVERABLE status, and you risk data loss\. For more information on the IRRECOVERABLE status, see [Understanding Tape Status Information in a VTL](managing-gateway-vtl.md#tape-status)\.

1. Shut down the gateway by following the approach described in the [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md) section\.

1. Detach the Amazon EBS volume from your Amazon EC2 instance\. For instructions, see [Detaching an Amazon EBS Volume from an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-detaching-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Delete the Amazon EBS volume\. For instructions, see [Deleting an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Start the gateway by following the approach described in the [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md) section\.