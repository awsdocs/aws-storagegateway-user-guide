--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Troubleshooting virtual tape issues<a name="Main_TapesIssues-vtl"></a>

You can find information following about actions to take if you experience unexpected issues with your virtual tapes\.

**Topics**
+ [Recovering a Virtual Tape From An Unrecoverable Gateway](#recovery-tapes)
+ [Troubleshooting Irrecoverable Tapes](#IrrecoverableTapes)
+ [High Availability Health Notifications](#troubleshooting-ha-notifications)

## Recovering a Virtual Tape From An Unrecoverable Gateway<a name="recovery-tapes"></a>

Although it is rare, your Tape Gateway might encounter an unrecoverable failure\. Such a failure can occur in your hypervisor host, the gateway itself, or the cache disks\. If a failure occurs, you can recover your tapes by following the troubleshooting instructions in this section\.

**Topics**
+ [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](#creating-recovery-tape-vtl)
+ [You Need to Recover a Virtual Tape from a Malfunctioning Cache Disk](#recover-from-failed-disk)

### You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway<a name="creating-recovery-tape-vtl"></a>

If your Tape Gateway or the hypervisor host encounters an unrecoverable failure, you can recover any data that has already been uploaded to AWS to another Tape Gateway\.

Note that the data written to a tape might not be completely uploaded until that tape has been successfully archived into VTS\. The data on tapes recovered to another gateway in this manner may be incomplete or empty\. We recommend performing an inventory on all recovered tapes to ensure they contain the expected content\. 

**To recover a tape to another Tape Gateway**

1. Identify an existing functioning Tape Gateway to serve as your recovery target gateway\. If you don't have a Tape Gateway to recover your tapes to, create a new Tape Gateway\. For information about how to create a gateway, see [Creating a Gateway](create-gateway-vtl.md)\. 

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the Tape Gateway you want to recover tapes from\.

1. Choose the **Details** tab\. A tape recovery message is displayed in the tab\.

1. Choose **Create recovery tapes** to disable the gateway\.

1. In the dialog box that appears, choose **Disable gateway**\.

   This process permanently halts normal function of your Tape Gateway and exposes any available recovery points\. For instructions, see [Disabling Your Tape Gateway](managing-gateway-vtl.md#disabling-gateway-vtl)\.

1. From the tapes that the disabled gateway displays, choose the virtual tape and the recovery point you want to recover\. A virtual tape can have multiple recovery points\.

1. To begin recovering any tapes you need to the target Tape Gateway, choose **Create recovery tape**\.

1. In the **Create recovery tape** dialog box, verify the barcode of the virtual tape you want to recover\.

1. For **Gateway**, choose the Tape Gateway you want to recover the virtual tape to\.

1. Choose **Create recovery tape**\. 

1. Delete the failed Tape Gateway so you don't get charged\. For instructions, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

Storage Gateway moves the tape from the failed Tape Gateway to the Tape Gateway you specified\. The Tape Gateway marks the tape status as RECOVERED\. 

### You Need to Recover a Virtual Tape from a Malfunctioning Cache Disk<a name="recover-from-failed-disk"></a>

If your cache disk encounters an error, the gateway prevents read and write operations on virtual tapes in the gateway\. For example, an error can occur when a disk is corrupted or removed from the gateway\. The Storage Gateway console displays a message about the error\. 

In the error message, Storage Gateway prompts you to take one of two actions that can recover your tapes:
+  **Shut Down and Re\-Add Disks **– Take this approach if the disk has intact data and has been removed\. For example, if the error occurred because a disk was removed from your host by accident but the disk and the data is intact, you can re\-add the disk\. To do this, see the procedure later in this topic\.
+  **Reset Cache Disk** – Take this approach if the cache disk is corrupted or not accessible\. If the disk error causes the cache disk to be inaccessible, unusable, or corrupted, you can reset the disk\. If you reset the cache disk, tapes that have clean data \(that is, tapes for which data in the cache disk and Amazon S3 are synchronized\) will continue to be available for you to use\. However, tapes that have data that is not synchronized with Amazon S3 are automatically recovered\. The status of these tapes is set to RECOVERED, but the tapes will be read\-only\. For information about how to remove a disk from your host, see [Determining the size of upload buffer to allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.
**Important**  
If the cache disk you are resetting contains data that has not been uploaded to Amazon S3 yet, that data can be lost\. After you reset cache disks, no configured cache disks will be left in the gateway, so you must configure at least one new cache disk for your gateway to function properly\.

  To reset the cache disk, see the procedure later in this topic\.

**To shut down and re\-add a disk**

1. Shut down the gateway\. For information about how to shut down a gateway, see [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md)\.

1. Add the disk back to your host, and make sure the disk node number of the disk has not changed\. For information about how to add a disk, see [Determining the size of upload buffer to allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.

1. Restart the gateway\. For information about how to restart a gateway, see [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md)\.

After the gateway restarts, you can verify the status of the cache disks\. The status of a disk can be one of the following:
+ **present** – The disk is available to use\.
+ **missing** – The disk is no longer connected to the gateway\.
+ **mismatch** – The disk node is occupied by a disk that has incorrect metadata, or the disk content is corrupted\.

**To reset and reconfigure a cache disk**

1. In the **A disk error has occurred** error message illustrated preceding, choose **Reset Cache Disk**\. 

1. On the **Configure gateway** page, configure the disk for cache storage\. For information about how to do so, see [Configure your Tape Gateway](create-gateway-vtl.md#configure-gateway-tape)\.

1. After you have configured cache storage, shut down and restart the gateway as described in the previous procedure\.

The gateway should recover after the restart\. You can then verify the status of the cache disk\.

**To verify the status of a cache disk**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose your gateway\.

1. For **Actions**, choose **Configure Local Storage** to display the **Configure Local Storage** dialog box\. This dialog box shows all local disks in the gateway\.

The cache disk node status is displayed next to the disk\.

**Note**  
If you don't complete the recovery process, the gateway displays a banner that prompts you to configure local storage\.

## Troubleshooting Irrecoverable Tapes<a name="IrrecoverableTapes"></a>

If your virtual tape fails unexpectedly, Storage Gateway sets the status of the failed virtual tape to IRRECOVERABLE\. The action you take depends on the circumstances\. You can find information following on some issues you might find, and how to troubleshoot them\.

### You Need to Recover Data From an IRRECOVERABLE Tape<a name="IrrecoverableTapes.NeedTape"></a>

If you have a virtual tape with the status IRRECOVERABLE, and you need to work with it, try one of the following: 
+ Activate a new Tape Gateway if you don't have one activated\. For more information, see [Creating a Gateway](create-gateway-vtl.md)\.
+ Disable the Tape Gateway that contains the irrecoverable tape, and recover the tape from a recovery point to the new Tape Gateway\. For more information, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](#creating-recovery-tape-vtl)\.
**Note**  
You have to reconfigure your iSCSI initiator and backup application to use the new Tape Gateway\. For more information, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\. 

### You Don't Need an IRRECOVERABLE Tape That Isn't Archived<a name="IrrecoverableTapes.DoNotNeedNotArchived"></a>

If you have a virtual tape with the status IRRECOVERABLE, you don't need it, and the tape has never been archived, you should delete the tape\. For more information, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\. 

### A Cache Disk in Your Gateway Encounters a Failure<a name="IrrecoverableTapes.CacheFails"></a>

If one or more cache disks in your gateway encounters a failure, the gateway prevents read and write operations to your virtual tapes and volumes\. To resume normal functionality, reconfigure your gateway as described following:
+ If the cache disk is inaccessible or unusable, delete the disk from your gateway configuration\.
+ If the cache disk is still accessible and useable, reconnect it to your gateway\.

**Note**  
If you delete a cache disk, tapes or volumes that have clean data \(that is, for which data in the cache disk and Amazon S3 are synchronized\) will continue to be available when the gateway resumes normal functionality\. For example, if your gateway has three cache disks and you delete two, tapes or volumes that are clean will have AVAILABLE status\. Other tapes and volumes will have IRRECOVERABLE status\.  
If you use ephemeral disks as cache disks for your gateway or mount your cache disks on an ephemeral drive, your cache disks will be lost when you shut down the gateway\. Shutting down the gateway when your cache disk and Amazon S3 are not synchronized can result in data loss\. As a result, we don't recommend using ephemeral drives or disks\.

## High Availability Health Notifications<a name="troubleshooting-ha-notifications"></a>

When running your gateway on the VMware vSphere High Availability \(HA\) platform, you may receive health notifications\. For more information about health notifications, see [Troubleshooting high availability issues](troubleshooting-ha-issues.md)\.