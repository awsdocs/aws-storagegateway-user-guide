# Testing Your Setup by Using Veritas Backup Exec<a name="backup-BackupExec"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using Veritas Backup Exec\. In this topic, you can find basic documentation needed to perform backup and restore operations using the following versions of Backup Exec: 
+ Veritas Backup Exec 2014
+ Veritas Backup Exec 15
+ Veritas Backup Exec 16
+ Veritas Backup Exec 20\.x

The procedure for using these versions of Backup Exec with a tape gateway is the same\. For detailed information about how to use Backup Exec, see the [How to Create Secure Backups with Backup Exec](http://www.symantec.com/tv/products/details.jsp?vid=3517643941001&subcategory=backupexec&pid=1) video on the Backup Exec website\. For Backup Exec support information on hardware compatibility, see the [ Software Compatibility Lists \(SCL\), Hardware Compatibility Lists \(HCL\), and Administrator Guides for Backup Exec \(all versions\) ](http://www.symantec.com/business/support/index?page=content&id=TECH205797) on the Backup Exec website\. For information about best practices, see [ Best Practices for using Symantec Backup products \(NetBackup, Backup Exec\) with the Amazon Web Services \(Tape Gateway\)](https://support.symantec.com/en_US/article.TECH227133.html) on the Veritas website\. 

For more information about supported backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.

**Topics**
+ [Configuring Storage in Backup Exec](#BE-configure-storage)
+ [Importing a Tape in Backup Exec](#BE-import-tape)
+ [Writing Data to a Tape in Backup Exec](#BE-write-data-to-tape)
+ [Archiving a Tape Using Backup Exec](#BE-archive-tapes)
+ [Restoring Data from a Tape Archived in Backup Exec](#BE-restore-tape)
+ [Disabling a Tape Drive in Backup Exec](#BE-disable-tape-drive)

## Configuring Storage in Backup Exec<a name="BE-configure-storage"></a>

After you have connected the virtual tape library \(VTL\) devices to the Windows client, you configure Backup Exec storage to recognize your devices\. For information about how to connect VTL devices to the Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

**To configure storage**

1. Start the Backup Exec software, and then choose the yellow icon in top\-left corner on the toolbar\.

1. Choose **Configuration and Settings**, and then choose **Backup Exec Services** to open the Backup Exec Service Manager\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE14MenuIcon2.png)

1. Choose **Restart All Services**\. Backup Exec then recognizes the VTL devices \(that is, the medium changer and tape drives\)\. The restart process might take a few minutes\.
**Note**  
Tape Gateway provides 10 tape drives\. However, your Backup Exec license agreement might require your backup application to work with fewer than 10 tape drives\. In that case, you must disable tape drives in the Backup Exec robotic library to leave only the number of tape drives allowed by your license agreement enabled\. For instructions, see [Disabling a Tape Drive in Backup Exec ](#BE-disable-tape-drive)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BEServiceManager2.png)

1. After the restart is completed, close the Backup Exec Service Manager\.

## Importing a Tape in Backup Exec<a name="BE-import-tape"></a>

You are now ready to import a tape from your gateway into a slot\.

1. Choose the **Storage** tab, and then expand the **Robotic library** tree to display the VTL devices\. 
**Important**  
Veritas Backup Exec software requires the Tape Gateway medium changer type\. If the medium changer type listed under **Robotic library** is not Tape Gateway, you must change it before you configure storage in the backup application\. For information about how to select a different medium changer type, see [Selecting a Medium Changer After Gateway Activation](resource_vtl-devices.md#change-mediumchanger-vtl)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE14ShowVTLDevices2.png)

1. Choose the **Slots** icon to display all slots\. 
**Note**  
When you import tapes into the robotic library, the tapes are stored in slots instead of tape drives\. Therefore, the tape drives might have a message that indicates there is no media in the drives \(No media\)\. When you initiate a backup or restore job, the tapes are moved into the tape drives\.  
You must have tapes available in your gateway tape library to import a tape into a storage slot\. For instructions on how to create tapes, see [Adding Virtual Tapes](managing-gateway-vtl.md#creating-virtual-tapes-vtl)\.

1. Open the context \(right\-click\) menu for an empty slot, choose **Import**, and then choose **Import media now**\. In the following screenshot, slot number **3** is empty\. You can select more than one slot and import multiple tapes in a single import operation\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE14-import-media-now2.png)

1. In the **Media Request** window that appears, choose **View details**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE14-DetailsLink2.png)

1. In the **Action Alert: Media Intervention** window, choose **Respond OK** to insert the media into the slot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE-ResponseOK2.png)

   The tape appears in the slot you selected\.
**Note**  
Tapes that are imported include empty tapes and tapes that have been retrieved from the archive to the gateway\.

## Writing Data to a Tape in Backup Exec<a name="BE-write-data-to-tape"></a>

You write data to a tape gateway virtual tape by using the same procedure and backup policies you do with physical tapes\. For detailed information, see the *Backup Exec Administrative Guide *in the documentation section in the Backup Exec software\.

## Archiving a Tape Using Backup Exec<a name="BE-archive-tapes"></a>

When you archive a tape, tape gateway moves the tape from your gateway’s virtual tape library \(VTL\) to the offline storage\. You begin tape archival by exporting the tape using your Backup Exec software\.

**To archive your tape**

1. Choose the **Storage** menu, choose **Slots**, open the context \(right\-click\) menu for the slot you want to export the tape from, choose **Export media**, and then choose **Export media now**\. You can select more than one slot and export multiple tapes in a single export operation\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BE14-ExportMedia2.png)

1. In the **Media Request** pop\-up window, choose **View details**, and then choose **Respond OK** in the **Alert: Media Intervention** window\. 

   In the AWS Storage Gateway console, you can verify the status of the tape you are archiving\. It might take some time to finish uploading data to AWS\. During this time, the exported tape is listed in the tape gateway's VTL with the status **IN TRANSIT TO VTS**\. When the upload is completed and the archiving process begins, the status changes to **ARCHIVING**\. When data archiving has completed, the exported tape is no longer listed in the VTL\.

1. Choose your gateway, and then choose **VTL Tape Cartridges** and verify that the virtual tape is no longer listed in your gateway\. 

1. On the Navigation pane of the AWS Storage Gateway console, choose **Tapes**\. Verify that your tapes status is ARCHIVED\.

## Restoring Data from a Tape Archived in Backup Exec<a name="BE-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape to a tape gateway\. For instructions, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use Backup Exec to restore the data\. This process is the same as restoring data from physical tapes\. For instructions, see the *Backup Exec Administrative Guide *in the documentation section in the Backup Exec software\.

## Disabling a Tape Drive in Backup Exec<a name="BE-disable-tape-drive"></a>

A tape gateway provides 10 tape drives, but you might decide to use fewer tape drives\. In that case, you disable the tape drives you don't use\.

1. Open Backup Exec, and choose the **Storage** tab\.

1. In the **Robotic library** tree, open the context \(right\-click\) menu for the tape drive you want to disable, and then choose **Disable**\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)