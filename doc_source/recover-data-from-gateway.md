--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Best practices for recovering your data<a name="recover-data-from-gateway"></a>

Although it is rare, your gateway might encounter an unrecoverable failure\. Such a failure can occur in your virtual machine \(VM\), the gateway itself, the local storage, or elsewhere\. If a failure occurs, we recommend that you follow the instructions in the appropriate section following to recover your data\.

**Important**  
Storage Gateway doesn’t support recovering a gateway VM from a snapshot that is created by your hypervisor or from your Amazon EC2 Amazon Machine Image \(AMI\)\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway using the instructions following\.

**Topics**
+ [Recovering from an unexpected virtual machine shutdown](#recover-from-gateway-shutdown)
+ [Recovering your data from a malfunctioning gateway or VM](#recover-from-gateway)
+ [Recovering your data from an irrecoverable volume](#recover-from-volume)
+ [Recovering your data from an irrecoverable tape](#recover-from-tape)
+ [Recovering your data from a malfunctioning cache disk](#recover-from-cahe-disk)
+ [Recovering your data from a corrupted file system](#recover-corrupt-file-system)
+ [Recovering your data from an inaccessible data center](#disaster-recovery)

## Recovering from an unexpected virtual machine shutdown<a name="recover-from-gateway-shutdown"></a>

If your VM shuts down unexpectedly, for example during a power outage, your gateway becomes unreachable\. When power and network connectivity are restored, your gateway becomes reachable and starts to function normally\. Following are some steps you can take at that point to help recover your data:
+ If an outage causes network connectivity issues, you can troubleshoot the issue\. For information about how to test network connectivity, see [Testing Your Gateway Connection to the Internet](manage-on-premises-common.md#MaintenanceTestGatewayConnectivity-common)\.
+  For cached volumes and tapes setups, when your gateway becomes reachable, your volumes or tapes go into BOOTSTRAPPING status\. This functionality ensures that your locally stored data continues to be synchronized with AWS\. For more information on this status, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\.
+ If your gateway malfunctions and issues occur with your volumes or tapes as a result of an unexpected shutdown, you can recover your data\. For information about how to recover your data, see the sections following that apply to your scenario\.

## Recovering your data from a malfunctioning gateway or VM<a name="recover-from-gateway"></a>

If your gateway or virtual machine malfunctions, you can recover data that has been uploaded to AWS and stored on a volume in Amazon S3\. For cached volumes gateways, you recover data from a recovery snapshot\. For stored volumes gateways, you can recover data from your most recent Amazon EBS snapshot of the volume\. For Tape Gateways, you recover one or more tapes from a recovery point to a new tape gateway\.

If your cached volumes gateway becomes unreachable, you can use the following steps to recover your data from a recovery snapshot:

1. In the AWS Management Console, choose the malfunctioning gateway, choose the volume you want to recover, and then create a recovery snapshot from it\.

1. Deploy and activate a new volume gateway\. Or, if you have an existing functioning volume gateway, you can use that gateway to recover your volume data\.

1. Find the snapshot you created and restore it to a new volume on the functioning gateway\.

1. Mount the new volume as an iSCSI device on your on\-premises application server\.

For detailed information on how to recover cached volumes data from a recovery snapshot, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\.

If your Tape Gateway or the hypervisor host encounters an unrecoverable failure, you can use the following steps to recover the tapes from the malfunctioning Tape Gateway to another Tape Gateway:

1. Identify the Tape Gateway that you want to use as the recovery target, or create a new one\.

1. Disable the malfunctioning gateway\.

1. Create recovery tapes for each tape that you want to recover and specify the target Tape Gateway\.

1. Delete the malfunctioning Tape Gateway\.

For detailed information on how to recover the tapes from a malfunctioning Tape Gateway to another Tape Gateway, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\.

## Recovering your data from an irrecoverable volume<a name="recover-from-volume"></a>

If the status of your volume is IRRECOVERABLE, you can no longer use this volume\.

For stored volumes, you can retrieve your data from the irrecoverable volume to a new volume by using the following steps:

1. Create a new volume from the disk that was used to create the irrecoverable volume\.

1. Preserve existing data when you are creating the new volume\.

1. Delete all pending snapshot jobs for the irrecoverable volume\.

1. Delete the irrecoverable volume from the gateway\.

For cached volumes, we recommend using the last recovery point to clone a new volume\.

For detailed information about how to retrieve your data from an irrecoverable volume to a new volume, see [The Console Says That Your Volume Is Irrecoverable](troubleshoot-volume-issues.md#troubleshoot-volume-issues.VolumeIrrecoverable)\.

## Recovering your data from an irrecoverable tape<a name="recover-from-tape"></a>

If your tape encounters a failure and the status of the tape is IRRECOVERABLE, we recommend you use one of the following options to recover your data or resolve the failure depending on your situation:
+ If you need the data on the irrecoverable tape, you can recover the tape to a new gateway\.
+ If you don't need the data on the tape, and the tape has never been archived, you can simply delete the tape from your Tape Gateway\.

   For detailed information about how to recover your data or resolve the failure if your tape is IRRECOVERABLE, see [Troubleshooting Irrecoverable Tapes](Main_TapesIssues-vtl.md#IrrecoverableTapes)\.

## Recovering your data from a malfunctioning cache disk<a name="recover-from-cahe-disk"></a>

If your cache disk encounters a failure, we recommend you use the following steps to recover your data depending on your situation:
+ If the malfunction occurred because a cache disk was removed from your host, shut down the gateway, re\-add the disk, and restart the gateway\.
+ If the cache disk is corrupted or not accessible, shut down the gateway, reset the cache disk, reconfigure the disk for cache storage, and restart the gateway\.

For detailed information, see [You Need to Recover a Virtual Tape from a Malfunctioning Cache Disk](Main_TapesIssues-vtl.md#recover-from-failed-disk)\.

## Recovering your data from a corrupted file system<a name="recover-corrupt-file-system"></a>

If your file system gets corrupted, you can use the **fsck** command to check your file system for errors and repair it\. If you can repair the file system, you can then recover your data from the volumes on the file system, as described following:

1. Shut down your virtual machine and use the Storage Gateway Management Console to create a recovery snapshot\. This snapshot represents the most current data stored in AWS\.
**Note**  
You use this snapshot as a fallback if your file system can't be repaired or the snapshot creation process can't be completed successfully\.

   For information about how to create a recovery snapshot, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\.

1. Use the **fsck** command to check your file system for errors and attempt a repair\.

1. Restart your gateway VM\.

1. When your hypervisor host starts to boot up, press and hold down shift key to enter the grub boot menu\.

1. From the menu, press **e** to edit\.

1. Choose the kernel line \(the second line\), and then press **e** to edit\.

1. Append the following option to the kernel command line: **init=/bin/bash**\. Use a space to separate the previous option from the option you just appended\.

1. Delete both `console=` lines, making sure to delete all values following the `=` symbol, including those separated by commas\.

1. Press **Return** to save the changes\.

1. Press **b** to boot your computer with the modified kernel option\. Your computer will boot to a `bash#` prompt\.

1. Enter **/sbin/fsck \-f */dev/sda1*** to run this command manually from the prompt, to check and repair your file system\. If the command does not work with the `/dev/sda1` path, you can use **lsblk** to determine the root filesystem device for `/` and use that path instead\.

1. When the file system check and repair is complete, reboot the instance\. The grub settings will revert to the original values, and the gateway will boot up normally\.

1. Wait for snapshots that are in\-progress from the original gateway to complete, and then validate the snapshot data\.

You can continue to use the original volume as\-is, or you can create a new gateway with a new volume based on either the recovery snapshot or the completed snapshot\. Alternatively, you can create a new volume from any of your completed snapshots from this volume\.

## Recovering your data from an inaccessible data center<a name="disaster-recovery"></a>

If your gateway or data center becomes inaccessible for some reason, you can recover your data to another gateway in a different data center or recover to a gateway hosted on an Amazon EC2 instance\. If you don't have access to another data center, we recommend creating the gateway on an Amazon EC2 instance\. The steps you follow depends on the gateway type you are covering the data from\.

**To recover data from a volume gateway in an inaccessible data center**

1. Create and activate a new volume gateway on an Amazon EC2 host\. For more information, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\.
**Note**  
Gateway stored volumes can't be hosted on Amazon EC2 instance\.

1. Create a new volume and choose the EC2 gateway as the target gateway\. For more information, see [Creating a volume](GettingStartedCreateVolumes.md)\.

   Create the new volume based on an Amazon EBS snapshot or clone from last recovery point of the volume you want to recover\.

   If your volume is based on a snapshot, provide the snapshot id\.

   If you are cloning a volume from a recovery point, choose the source volume\.

**To recover data from a tape gateway in an inaccessible data center**

1. Create and activate a new Tape Gateway on an Amazon EC2 host\. For more information, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\.

1. Recover the tapes from the source gateway in the data center to the new gateway you created on Amazon EC2 For more information, see [Recovering a Virtual Tape From An Unrecoverable Gateway](Main_TapesIssues-vtl.md#recovery-tapes)\.

   Your tapes should be covered to the new Amazon EC2 gateway\.

**To recover data from a file gateway in an inaccessible data center**

For file gateway, you map a new file share to the Amazon S3 bucket that contains the data you want to recover\.

1. Create and activate a new file gateway on an Amazon EC2 host\. For more information, see [Deploying a file gateway on an Amazon EC2 host](ec2-gateway-file.md)\.

1. Create a new file share on the EC2 gateway you created\. For more information, see [Create a file share](https://docs.aws.amazon.com/filegateway/latest/files3/GettingStartedCreateFileShare.html)\.

1. Mount your file share on your client and map it to the S3 bucket that contains the data that you want to recover\. For more information, see [Mount and use your file share](https://docs.aws.amazon.com/filegateway/latest/files3/getting-started-use-fileshare.html)\.