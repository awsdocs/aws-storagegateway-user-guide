# Testing Your Setup by Using Quest NetVault Backup<a name="backup-netvault"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using Quest \(formerly Dell\) NetVault Backup version 10\.0\. In this topic, you can find basic documentation on how to configure the Quest NetVault Backup application for a tape gateway and perform a backup and restore operation\. 

For additional setup information, see [Backing up to Amazon AWS with Quest NetVault Backup](https://www.scribd.com/document/294138486/backing-up-to-amazon-aws-with-dell-netvault-backup-technical-brief-75488-pdf) on the Quest \(formerly Dell\) website\. For detailed information about how to use the Quest NetVault Backup application, see the [Quest NetVault Backup 10\.0\.1 – Administration Guide](https://support.quest.com/technical-documents/netvault-backup/10.0.1/administration-guide/26#TOPIC-229714)\. For more information about compatible backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.

**Topics**
+ [Configuring Quest NetVault Backup to Work with VTL Devices](#netvault-configure-software)
+ [Backing Up Data to a Tape in the Quest NetVault Backup](#netvault-write-data-to-tape)
+ [Archiving a Tape by Using the Quest NetVault Backup](#netvault-archive-tape)
+ [Restoring Data from a Tape Archived in Quest NetVault Backup](#netvault-restore-tape)

## Configuring Quest NetVault Backup to Work with VTL Devices<a name="netvault-configure-software"></a>

After you have connected the virtual tape library \(VTL\) devices to the Windows client, you configure Quest NetVault Backup to recognize your devices\. For information about how to connect VTL devices to the Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

The Quest NetVault Backup application doesn't automatically recognize tape gateway devices\. You must manually add the devices to expose them to the Quest NetVault Backup application and then discover the VTL devices\.

### Adding VTL Devices<a name="netvault-add-devices"></a>

**To add the VTL devices**

1. In Quest NetVault Backup, choose **Manage Devices** in the **Configuration** tab\.

1. On the Manage Devices page, choose **Add Devices**\.

1. In the Add Storage Wizard, choose **Tape library / media changer**, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetVault-AddStorage.png)

1. On the next page, choose the client machine that is physically attached to the library and choose **Next** to scan for devices\.

1. If devices are found, they are displayed\. In this case, your medium changer is displayed in the device box\.

1. Choose your medium changer and choose **Next**\. Detailed information about the device is displayed in the wizard\.

1. On the Add Tapes to Bays page, choose **Scan For Devices**, choose your client machine, and then choose **Next**\. 

   All your drives are displayed on the page\. Quest NetVault Backup displays the 10 bays to which you can add your drives\. The bays are displayed one at a time\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetVault-AddDrives.png)

1. Choose the drive you want to add to the bay that is displayed, and then choose **Next**\.
**Important**  
When you add a drive to a bay, the drive and bay numbers must match\. For example, if bay 1 is displayed, you must add drive 1\. If a drive is not connected, leave its matching bay empty\.

1. When your client machine appears, choose it, and then choose **Next**\. The client machine can appear multiple times\.

1. When the drives are displayed, repeat steps 7 through 9 to add all the drives to the bays\.

1. In the **Configuration** tab, choose **Manage devices** and on the **Manage Devices** page, expand your medium changer to see the devices that you added\.

## Backing Up Data to a Tape in the Quest NetVault Backup<a name="netvault-write-data-to-tape"></a>

You create a backup job and write data to a virtual tape by using the same procedures you do with physical tapes\. For detailed information about how to back up data, see the [Quest NetVault Backup documentation](https://support.quest.com/technical-documents/netvault-backup/10.0.1/administration-guide/26#TOPIC-229714)\.

## Archiving a Tape by Using the Quest NetVault Backup<a name="netvault-archive-tape"></a>

When you archive a tape, a tape gateway ejects the tape from the tape drive to the storage slot\. It then exports the tape from the slot to the archive by using your backup application—that is, the Quest NetVault Backup\.

**To archive a tape in Quest NetVault Backup**

1. In the Quest NetVault Backup Configuration tab, choose and expand your medium changer to see your tapes\.

1. On the **Slots** row, choose the settings icon to open the **Slots Browser** for the medium changer\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetVault-Settings.png)

1. In the slots, locate the tape you want to archive, choose it, and then choose **Export**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/NetVault-ImportTapes.png)

## Restoring Data from a Tape Archived in Quest NetVault Backup<a name="netvault-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape from archive to a tape gateway\. For instructions, see [Retrieving Archived Tapes ](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use the Quest NetVault Backup application to restore the data\. You do this by creating a restoring a folder file, as you do when restoring data from physical tapes\. For instructions, see [Quest NetVault Backup 10\.0\.1 – Administration Guide \(Creating a restore job\)](http://support-public.cfm.quest.com/4b094d45-9eab-4ba5-8a57-09ed5f8e6840:1785915.pdf) in the Quest NetVault Backup documentation\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)