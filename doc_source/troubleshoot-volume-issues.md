--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Troubleshooting volume issues<a name="troubleshoot-volume-issues"></a>

You can find information about the most typical issues you might encounter when working with volumes, and actions that we suggest that you take to fix them\.

**Topics**
+ [The Console Says That Your Volume Is Not Configured](#troubleshoot-volume-issues.VolumeNotConfigured)
+ [The Console Says That Your Volume Is Irrecoverable](#troubleshoot-volume-issues.VolumeIrrecoverable)
+ [Your Cached Gateway is Unreachable And You Want to Recover Your Data](#RecoverySnapshotTroubleshooting)
+ [The Console Says That Your Volume Has PASS THROUGH Status](#troubleshoot-volume-issues.VolumePassthrough)
+ [You Want to Verify Volume Integrity and Fix Possible Errors](#troubleshoot-volume-issues.VerifyIntegrity)
+ [Your Volume's iSCSI Target Doesn’t Appear in Windows Disk Management Console](#troubleshoot-volume-issues.DoesNotAppear)
+ [You Want to Change Your Volume's iSCSI Target Name](#troubleshoot-volume-issues.ChangeISCSI)
+ [Your Scheduled Volume Snapshot Did Not Occur](#troubleshoot-volume-issues.NoSnapshot)
+ [You Need to Remove or Replace a Disk That Has Failed](#troubleshoot-volume-issues.RemoveVolume)
+ [Throughput from Your Application to a Volume Has Dropped to Zero](#troubleshoot-volume-issues.ThroughputZero)
+ [A Cache Disk in Your Gateway Encounters a Failure](#troubleshoot-volume-issues.CacheDiskFail)
+ [A Volume Snapshot Has PENDING Status Longer Than Expected](#SnapshotTroubleshooting.Pending)
+ [High Availability Health Notifications](#troubleshooting-ha-notifications)

## The Console Says That Your Volume Is Not Configured<a name="troubleshoot-volume-issues.VolumeNotConfigured"></a>

If the Storage Gateway console indicates that your volume has a status of UPLOAD BUFFER NOT CONFIGURED, add upload buffer capacity to your gateway\. You cannot use a gateway to store your application data if the upload buffer for the gateway is not configured\. For more information, see [To add and configure upload buffer or cache storage](ManagingLocalStorage-common.md#GatewayWorkingStorageCachedTaskBuffer)\.

## The Console Says That Your Volume Is Irrecoverable<a name="troubleshoot-volume-issues.VolumeIrrecoverable"></a>

For stored volumes, if the Storage Gateway console indicates that your volume has a status of IRRECOVERABLE, you can no longer use this volume\. You can try to delete the volume in the Storage Gateway console\. If there is data on the volume, then you can recover the data when you create a new volume based on the local disk of the VM that was initially used to create the volume\. When you create the new volume, select **Preserve existing data**\. Make sure to delete pending snapshots of the volume before deleting the volume\. For more information, see [Deleting a Snapshot](managing-volumes.md#DeletingASnapshot)\. If deleting the volume in the Storage Gateway console does not work, then the disk allocated for the volume might have been improperly removed from the VM and cannot be removed from the appliance\.

For cached volumes, if the Storage Gateway console indicates that your volume has a status of IRRECOVERABLE, you can no longer use this volume\. If there is data on the volume, you can create a snapshot of the volume and then recover your data from the snapshot or you can clone the volume from the last recovery point\. You can delete the volume after you have recovered your data\. For more information, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](#RecoverySnapshotTroubleshooting)\.

For stored volumes, you can create a new volume from the disk that was used to create the irrecoverable volume\. For more information, see [Creating a volume](GettingStartedCreateVolumes.md)\. For information about volume status, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\. 

## Your Cached Gateway is Unreachable And You Want to Recover Your Data<a name="RecoverySnapshotTroubleshooting"></a>

When your gateway becomes unreachable \(such as when you shut it down\), you have the option of either creating a snapshot from a volume recovery point and using that snapshot, or cloning a new volume from the last recovery point for an existing volume\. Cloning from a volume recovery point is faster and more cost effective than creating a snapshot\. For more information about cloning a volume, see [Cloning a Volume](managing-volumes.md#clone-volume)\. 

Storage Gateway provides recovery points for each volume in a cached volume gateway architecture\. A *volume recovery point* is a point in time at which all data of the volume is consistent and from which you can create a snapshot or clone a volume\.

## The Console Says That Your Volume Has PASS THROUGH Status<a name="troubleshoot-volume-issues.VolumePassthrough"></a>

In some cases, the Storage Gateway console might indicate that your volume has a status of PASSTHROUGH\. A volume can have PASSTHROUGH status for several reasons\. Some reasons require action, and some do not\. 

An example of when you should take action if your volume has the PASS THROUGH status is when your gateway has run out of upload buffer space\. To verify if your upload buffer was exceeded in the past, you can view the `UploadBufferPercentUsed` metric in the Amazon CloudWatch console; for more information, see [Monitoring the upload buffer](Main_monitoring-gateways-common.md#PerfUploadBuffer-common)\. If your gateway has the PASS THROUGH status because it has run out of upload buffer space, you should allocate more upload buffer space to your gateway\. Adding more buffer space will cause your volume to transition from PASS THROUGH to BOOTSTRAPPING to AVAILABLE automatically\. While the volume has the BOOTSTRAPPING status, the gateway reads data off the volume's disk, uploads this data to Amazon S3, and catches up as needed\. When the gateway has caught up and saved the volume data to Amazon S3, the volume status becomes AVAILABLE and snapshots can be started again\. Note that when your volume has the PASS THROUGH or BOOTSTRAPPING status, you can continue to read and write data from the volume disk\. For more information about adding more upload buffer space, see [Determining the size of upload buffer to allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.

To take action before the upload buffer is exceeded, you can set a threshold alarm on a gateway's upload buffer\. For more information, see [To set an upper threshold alarm for a gateway's upload buffer](Main_monitoring-gateways-common.md#GatewayAlarm1-common)\. 

In contrast, an example of not needing to take action when a volume has the PASS THROUGH status is when the volume is waiting to be bootstrapped because another volume is currently being bootstrapped\. The gateway bootstraps volumes one at a time\.

Infrequently, the PASS THROUGH status can indicate that a disk allocated for an upload buffer has failed\. In this is the case, you should remove the disk\. For more information, see [Volume Gateway](resource-volume-gateway.md)\. For information about volume status, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\. 

## You Want to Verify Volume Integrity and Fix Possible Errors<a name="troubleshoot-volume-issues.VerifyIntegrity"></a>

If you want to verify volume integrity and fix possible errors, and your gateway uses Microsoft Windows initiators to connect to its volumes, you can use the Windows CHKDSK utility to verify the integrity of your volumes and fix any errors on the volumes\. Windows can automatically run the CHKDSK tool when volume corruption is detected, or you can run it yourself\. 

## Your Volume's iSCSI Target Doesn’t Appear in Windows Disk Management Console<a name="troubleshoot-volume-issues.DoesNotAppear"></a>

If your volume's iSCSI target does not show up in the Disk Management Console in Windows, check that you have configured the upload buffer for the gateway\. For more information, see [To add and configure upload buffer or cache storage](ManagingLocalStorage-common.md#GatewayWorkingStorageCachedTaskBuffer)\.

## You Want to Change Your Volume's iSCSI Target Name<a name="troubleshoot-volume-issues.ChangeISCSI"></a>

If you want to change the iSCSI target name of your volume, you must delete the volume and add it again with a new target name\. If you do so, you can preserve the data on the volume\. 

## Your Scheduled Volume Snapshot Did Not Occur<a name="troubleshoot-volume-issues.NoSnapshot"></a>

If your scheduled snapshot of a volume did not occur, check whether your volume has the PASSTHROUGH status, or if the gateway's upload buffer was filled just prior to the scheduled snapshot time\. You can check the `UploadBufferPercentUsed` metric for the gateway in the Amazon CloudWatch console and create an alarm for this metric\. For more information, see [Monitoring the upload buffer](Main_monitoring-gateways-common.md#PerfUploadBuffer-common) and [To set an upper threshold alarm for a gateway's upload buffer](Main_monitoring-gateways-common.md#GatewayAlarm1-common)\.

## You Need to Remove or Replace a Disk That Has Failed<a name="troubleshoot-volume-issues.RemoveVolume"></a>

If you need to replace a volume disk that has failed or replace a volume because it isn't needed, you should remove the volume first using the Storage Gateway console\. For more information, see [To remove a volume](managing-volumes.md#CachedRemovingAStorageVolume)\. You then use the hypervisor client to remove the backing storage:

 
+ For VMware ESXi, remove the backing storage as described in [Deleting a Volume](managing-volumes.md#ApplicationStorageVolumesCached-Removing)\.
+ For Microsoft Hyper\-V, remove the backing storage\.

## Throughput from Your Application to a Volume Has Dropped to Zero<a name="troubleshoot-volume-issues.ThroughputZero"></a>

If throughput from your application to a volume has dropped to zero, try the following:
+ If you are using the VMware vSphere client, check that your volume's **Host IP** address matches one of the addresses that appears in the vSphere client on the **Summary** tab\. You can find the **Host IP** address for a storage volume in the Storage Gateway console in the **Details** tab for the volume\. A discrepancy in the IP address can occur, for example, when you assign a new static IP address to your gateway\. If there is a discrepancy, restart your gateway from the Storage Gateway console as shown in [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md)\. After the restart, the **Host IP** address in the **ISCSI Target Info** tab for a storage volume should match an IP address shown in the vSphere client on the **Summary** tab for the gateway\. 
+ If there is no IP address in the **Host IP** box for the volume and the gateway is online\. For example, this could occur if you create a volume associated with an IP address of a network adapter of a gateway that has two or more network adapters\. When you remove or disable the network adapter that the volume is associated with, the IP address might not appear in the **Host IP** box\. To address this issue, delete the volume and then re\-create it preserving its existing data\.
+ Check that the iSCSI initiator your application uses is correctly mapped to the iSCSI target for the storage volume\. For more information about connecting to storage volumes, see [Connecting to Your Volumes to a Windows Client](initiator-connection-common.md#ConfiguringiSCSIClient)\.

You can view the throughput for volumes and create alarms from the Amazon CloudWatch console\. For more information about measuring throughput from your application to a volume, see [Measuring Performance Between Your Application and Gateway](monitoring-volume-gateway.md#PerfAppGateway-common)\.

## A Cache Disk in Your Gateway Encounters a Failure<a name="troubleshoot-volume-issues.CacheDiskFail"></a>

If one or more cache disks in your gateway encounters a failure, the gateway prevents read and write operations to your virtual tapes and volumes\. To resume normal functionality, reconfigure your gateway as described following:
+ If the cache disk is inaccessible or unusable, delete the disk from your gateway configuration\.
+ If the cache disk is still accessible and useable, reconnect it to your gateway\.

**Note**  
If you delete a cache disk, tapes or volumes that have clean data \(that is, for which data in the cache disk and Amazon S3 are synchronized\) will continue to be available when the gateway resumes normal functionality\. For example, if your gateway has three cache disks and you delete two, tapes or volumes that are clean will have AVAILABLE status\. Other tapes and volumes will have IRRECOVERABLE status\.  
If you use ephemeral disks as cache disks for your gateway or mount your cache disks on an ephemeral drive, your cache disks will be lost when you shut down the gateway\. Shutting down the gateway when your cache disk and Amazon S3 are not synchronized can result in data loss\. As a result, we don't recommend using ephemeral drives or disks\.

## A Volume Snapshot Has PENDING Status Longer Than Expected<a name="SnapshotTroubleshooting.Pending"></a>

If a volume snapshot remains in PENDING state longer than expected, the gateway VM might have crashed unexpectedly or the status of a volume might have changed to PASS THROUGH or IRRECOVERABLE\. If any of these are the case, the snapshot remains in PENDING status and the snapshot does not successfully complete\. In these cases, we recommend that you delete the snapshot\. For more information, see [Deleting a Snapshot](managing-volumes.md#DeletingASnapshot)\.

When the volume returns to AVAILABLE status, create a new snapshot of the volume\. For information about volume status, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\.

## High Availability Health Notifications<a name="troubleshooting-ha-notifications"></a>

When running your gateway on the VMware vSphere High Availability \(HA\) platform, you may receive health notifications\. For more information about health notifications, see [Troubleshooting high availability issues](troubleshooting-ha-issues.md)\.