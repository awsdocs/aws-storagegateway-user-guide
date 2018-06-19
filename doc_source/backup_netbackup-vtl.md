# Testing Your Setup by Using Veritas NetBackup Version 7\.x<a name="backup_netbackup-vtl"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using Veritas NetBackup version 7\.x\. In this topic, you can find basic documentation on how to configure the NetBackup application for a tape gateway and perform a backup and restore operation\. For detailed information about how to use NetBackup, see the [Veritas Services and Operations Readiness Tools \(SORT\)](https://sort.veritas.com/documents) on the Veritas website\. For Veritas support information on hardware compatibility, see the [NetBackup 7\.0 \- 7\.6\.x Hardware Compatibility List](https://www.veritas.com/support/en_US/article.000040791) on the Veritas website\.

For more information about compatible backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.

**Topics**
+ [Configuring NetBackup Storage Devices](#configure-netback-storage-devices)
+ [Backing Up Data to a Tape](#GettingStarted-backup-data-VTL)
+ [Archiving the Tape](#GettingStarted-archiving-tapes-vtl)
+ [Restoring Data from the Tape](#restore-data-vtl)

## Configuring NetBackup Storage Devices<a name="configure-netback-storage-devices"></a>

After you have connected the virtual tape library \(VTL\) devices to the Windows client, you configure Veritas NetBackup version 7\.x storage to recognize your devices\. For information about how to connect VTL devices to the Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

**To configure NetBackup to use storage devices on your tape gateway**

1. Open the NetBackup Administration Console and run it as an administrator\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetbackupConsole1-vtl.png)

1. Choose **Configure Storage Devices** to open the Device Configuration wizard\.

1. Choose **Next**\. The NetBackup application detects your computer as a device host\.

1. In the **Device Hosts** column, select your computer, and then choose **Next**\. The NetBackup application scans your computer for devices and discovers all devices\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupDeviceHost-vtl.png)

1. In the **Scanning Hosts** page, choose **Next**, and then choose **Next**\. The NetBackup application finds all 10 tape drives and the medium changer on your computer\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackUpDevicesFound-vtl.png)

1. In the **Backup Devices** window, choose **Next**\.

1. In the **Drag and Drop Configuration** window, verify that your medium changer is selected, and then choose **Next\.** 

1. In the dialog box that appears, choose **Yes** to save the configuration on your computer\. The NetBackup application updates the device configuration\.

1. When the update is completed, choose **Next** to make the devices available to the NetBackup application\. 

1. In the **Finished\!** window, choose **Finish**\.

**To verify your devices in the NetBackup application**

1. In the NetBackup Administration Console, expand the **Media and Device Management** node, and then expand the **Devices** node\. Choose **Drives** to display all the tape drives\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupAllDrives-vtl.png)

1. In the **Devices** node, choose **Robots** to display all your medium changers\. In the NetBackup application, the medium changer is called a *robot*\.

1. In the **All Robots** pane, open the context \(right\-click\) menu for **TLD\(0\)** \(that is, your robot\), and then choose **Inventory Robot**\. 

1. In the **Robot Inventory** window, verify that your host is selected from the **Device\-Host** list located in the **Select robot** category\.

1. Verify that your robot is selected from the **Robot** list\.

1. In the **Robot Inventory** window, select **Update volume configuration**, select **Preview changes**, select **Empty media access port prior to update**, and then choose **Start**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupUpdateConfig-vtl.png)

   The process then inventories your medium changer and virtual tapes in the NetBackup Enterprise Media Management \(EMM\) database\. NetBackup stores media information, device configuration, and tape status in the EMM\.

1. In the **Robot Inventory** window, choose **Yes** once the inventory is complete\. Choosing **Yes** here updates the configuration and moves virtual tapes found in import/export slots to the virtual tape library\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupUpdateInventoryResults-vtl.png)

   For example, the following screenshot shows three virtual tapes found in the import/export slots\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupUpdateConfig2-vtl.png)

1. Close the **Robot Inventory** window\.

1. In the **Media** node, expand the **Robots** node and choose **TLD\(0\)** to show all virtual tapes that are available to your robot \(medium changer\)\.
**Note**  
If you have previously connected other devices to the NetBackup application, you might have multiple robots\. Make sure that you select the right robot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupAllTapes-vtl.png)

Now that you have connected your devices and made them available to your backup application, you are ready to test your gateway\. To test your gateway, you back up data onto the virtual tapes you created and archive the tapes\. 

## Backing Up Data to a Tape<a name="GettingStarted-backup-data-VTL"></a>

You test the tape gateway setup by backing up data onto your virtual tapes\.

**Note**  
You should back up only a small amount of data for this Getting Started exercise, because there are costs associated with storing, archiving, and retrieving data\. For pricing information, see [Pricing](http://aws.amazon.com/storagegateway/#pricing) on the AWS Storage Gateway detail page\.

**To create a volume pool**

A *volume pool* is a collection of virtual tapes to use for a backup\.

1. Start the NetBackup Administration Console\.

1. Expand the **Media** node, open the context \(right\-click\) menu for **Volume Pool**, and then choose **New**\. The **New Volume Pool** dialog box appears\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupNewVolumePool-vtl.png)

1. For **Name**, type a name for your volume pool\.

1. For **Description**, type a description for the volume pool, and then choose **OK**\. The volume pool you just created is added to the volume pool list\.

   The following screenshot shows a list of volume pools\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupVolumePoolList-vtl.png)

**To add virtual tapes to a volume pool**

1. Expand the **Robots** node, and select the **TLD\(0\)** robot to display the virtual tapes this robot is aware of\.

   If you have previously connected a robot, your tape gateway robot might have a different name\.

1. From the list of virtual tapes, open the context \(right\-click\) menu for the tape you want to add to the volume pool, and choose **Change** to open the **Change Volumes** dialog box\. The following screenshot shows the **Change Volumes** dialog box\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupVolumePool2.png)

1. For **Volume Pool**, choose **New pool**\.

1. For **New pool**, select the pool you just created, and then choose **OK**\.

   You can verify that your volume pool contains the virtual tape that you just added by expanding the **Media** node and choosing your volume pool\.

**To create a backup policy**

The backup policy specifies what data to back up, when to back it up, and which volume pool to use\.

1. Choose your **Master Server** to return to the Vertias NetBackup console\.

   The following screenshot shows the NetBackup console with **Create a Policy** selected\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupCreatePolicy.png)

1. Choose **Create a Policy** to open the **Policy Configuration Wizard** window\.

1. Select **File systems, databases, applications**, and choose **Next**\.

1. For **Policy Name**, type a name for your policy and verify that **MS\-Windows** is selected from the **Select the policy type** list, and then choose **Next**\.

1. In the **Client List** window, choose **Add**, type the host name of your computer in the **Name** column, and then choose **Next**\. This step applies the policy you are defining to localhost \(your client computer\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupLocalhost-VTL.png)

1. In the **Files** window, choose **Add**, and then choose the folder icon\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupSpecifyFolder-VTL.png)

1. In the **Browse** window, browse to the folder or files you want to back up, choose **OK**, and then choose **Next**\.

1. In the **Backup Types** window, accept the defaults, and then choose **Next**\.
**Note**  
If you want to initiate the backup yourself, select **User Backup**\. 

1. In the **Frequency and Retention** window, select the frequency and retention policy you want to apply to the backup\. For this exercise, you can accept all the defaults and choose** Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupFreqRetention-VTL.png)

1. In the **Start** window, select **Off hours**, and then choose **Next**\. This selection specifies that your folder should be backed up during off hours only\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupOffHours-VTL2.png)

1. In the **Policy Configuration** wizard, choose **Finish**\.

The policy runs the backups according to the schedule\. You can also perform a manual backup at any time, which we do in the next step\.

**To perform a manual backup**

1. On the navigation pane of the NetBackup console, expand the **NetBackup Management** node\.

1. Expand the **Policies** node\.

1. Open the context \(right\-click\) menu for your policy, and choose **Manual Backup**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupManualBackup-vtl.png)

1. In the **Manual Backup** window, select a schedule, select a client, and then choose **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupManualBackupScheduleClient-vtl.png)

1. In the **Manual Backup Started** dialog box that appears, choose **OK**\.

1. On the navigation pane, choose **Activity Monitor** to view the status of your backup in the **Job ID** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupActivityMonitor-vtl.png)

To find the barcode of the virtual tape where NetBackup wrote the file data during the backup, look in the **Job Details** window as described in the following procedure\. You need this barcode in the procedure in the next section, where you archive the tape\.

**To find the barcode of a tape**

1. In **Activity Monitor**, open the context \(right\-click\) menu for the identifier of your backup job in the **Job ID** column, and then choose **Details**\. 

1. In the **Job Details** window, choose the **Detailed Status** tab\. 

1. In the **Status** box, locate the media ID\. For example, in the following screenshot, the media ID is **87A222**\. This ID helps you determine which tape you have written data to\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupBackupMediaID-vtl.png)

You have now successfully deployed a tape gateway, created virtual tapes, and backed up your data\. Next, you can archive the virtual tapes and retrieve them from the archive\.

## Archiving the Tape<a name="GettingStarted-archiving-tapes-vtl"></a>

When you archive a tape, tape gateway moves the tape from your gateway’s virtual tape library \(VTL\) to the archive, which provides offline storage\. You initiate tape archival by ejecting the tape using your backup application\.   

**To archive a virtual tape**

1. In the NetBackup Administration console, expand the **Media and Device Management** node, and expand the **Media** node\.

1. Expand **Robots** and choose **TLD**\(0\)\. 

1. Open the context \(right\-click\) menu for the virtual tape you want to archive, and choose **Eject Volume From Robot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupEjectVolume-vtl.png)

1. In the **Eject Volumes** window, make sure the **Media ID** matches the virtual tape you want to eject, and then choose **Eject**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupEjectedTape-vtl.png)

1. In the dialog box, choose **Yes**\. The dialog box is shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupRemoveEjectedMedia-vtl.png)

   When the eject process is completed, the status of the tape in the **Eject Volumes** dialog box indicates that the eject succeeded\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetBackupEjectSuccess-vtl.png)

1. Choose **Close** to close the **Eject Volumes** window\.

1. In the AWS Storage Gateway console, verify the status of the tape you are archiving in the gateway's VTL\. It can take some time to finish uploading data to AWS\. During this time, the ejected tape is listed in the gateway's VTL with the status **IN TRANSIT TO VTS**\. When archiving starts, the status is **ARCHIVING**\. Once data upload has completed, the ejected tape is no longer listed in the VTL\.

1. To verify that the virtual tape is no longer listed in your gateway, choose your gateway, and then choose **VTL Tape Cartridges**\. 

1. In the navigation pane of the AWS Storage Gateway console, choose **Tapes**\. Verify that your archived tape's status is **ARCHIVED**\.

## Restoring Data from the Tape<a name="restore-data-vtl"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape to a tape gateway\. For instructions, see [Retrieving Archived Tapes ](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use the Backup, Archive, and Restore software installed with the Veritas NetBackup application\. This process is the same as restoring data from physical tapes\. For instructions, see [Veritas Services and Operations Readiness Tools \(SORT\)](https://sort.veritas.com/documents) on the Veritas website\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)
