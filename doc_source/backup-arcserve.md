# Testing Your Setup by Using Arcserve Backup r17\.0<a name="backup-arcserve"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using Arcserve Backup r17\.0\. In this topic, you can find basic documentation to configure Arcserve Backup with a tape gateway and perform a backup and restore operation\. For detailed information about to use Arcserve Backup r17\.0, see [Arcserve Backup r17 documentation](https://documentation.arcserve.com/Arcserve-Backup/Available/R17/ENU/Bookshelf_Files/HTML/admingde/index.htm) in the *Arcserve Administration Guide\. *

The following screenshot shows the Arcserve menus\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/LoadTape-scan.png)


+ [Configuring Arcserve to Work with VTL Devices](#archServe-configure-software)
+ [Loading Tapes into a Media Pool](#archServe-load-tapes)
+ [Backing Up Data to a Tape](#archServe-backup-data)
+ [Archiving a Tape](#archServe-archive-tape)
+ [Restoring Data from a Tape](#archServe-restore-tape)

## Configuring Arcserve to Work with VTL Devices<a name="archServe-configure-software"></a>

After you have connected your virtual tape library \(VTL\) devices to your client, you scan for your devices\.

**To scan for VTL devices**

1. In the Arcserve Backup Manager, choose the **Utilities** menu\.

1. Choose **Media Assure and Scan**\.

## Loading Tapes into a Media Pool<a name="archServe-load-tapes"></a>

When the Arcserve software connects to your gateway and your tapes become available, Arcserve automatically loads your tapes\. If your gateway is not found in the Arcserve software, try restarting the tape engine in Arcserve\.

**To restart the tape engine**

1. Choose **Quick Start**, choose **Administration**, and then choose **Device**\.

1. On the navigation menu, open the context \(right\-click\) menu for your gateway and choose an import/export slot\.

1. Choose **Quick Import** and assign your tape to an empty slot\.

1. Open the context \(right\-click\) menu for your gateway and choose **Inventory/Offline Slots**\.

1. Choose **Quick Inventory** to retrieve media information from the database\.

If you add a new tape, you need to scan your gateway for the new tape to have it appear in Arcserve\. If the new tapes don't appear, you must import the tapes\.

**To import tapes**

1. Choose the **Quick Start** menu, choose **Back up**, and then choose **Destination tap**\.

1. Choose your gateway, open the context \(right\-click\) menu for one tape, and then choose **Import/Export Slot**\.

1. Open the context \(right\-click\) menu for each new tape and choose **Inventory**\.

1. Open the context \(right\-click\) menu for each new tape and choose **Format**\.

Each tape's barcode now appears in your Storage Gateway console, and each tape is ready to use\.

## Backing Up Data to a Tape<a name="archServe-backup-data"></a>

When your tapes have been loaded into Arcserve, you can back up data\. The backup process is the same as backing up physical tapes\.

**To back up data to a tape**

1. From the **Quick Start** menu, open the restore a backup session\.

1. Choose the **Source** tab, and then choose the file system or database system that you want to back up\.

1. Choose the **Schedule** tab and choose the repeat method you want to use\.

1. Choose the **Destination** tab and then choose the tape you want to use\. If the data you are backing up is larger than the tape can hold, Arcserve prompts you to mount a new tape\.

1. Choose **Submit** to back up your data\.

## Archiving a Tape<a name="archServe-archive-tape"></a>

When you archive a tape, your tape gateway moves the tape from the tape library to the offline storage\. Before you eject and archive a tape, you might want to check the content on it\.

**To archive a tape**

1. From the **Quick Start** menu, open the restore a backup session\.

1. Choose the **Source** tab, and then choose the file system or database system you want to back up\.

1. Choose the **Schedule** tab and choose the repeat method you want to use\.

1. Choose your gateway, open the context \(right\-click\) menu for one tape, and then choose **Import/Export Slot**\.

1. Assign a mail slot to load the tape\. The status in the Storage Gateway console changes to **Archive**\. The archive process might take some time\.

## Restoring Data from a Tape<a name="archServe-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape to a tape gateway\. For instructions, see [Retrieving Archived Tapes ](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use Arcserve to restore the data\. This process is the same as restoring data from physical tapes\. For instructions, see the [Arcserve Backup r17 documentation](https://documentation.arcserve.com/Arcserve-Backup/Available/R17/ENU/Bookshelf_Files/HTML/admingde/index.htm)\. 

To restore data from a tape, use the following procedure\.

**To restore data from a tape**

1. From the **Quick Start** menu, open the restore a restore session\.

1. Choose the **Source** tab, and then choose the file system or database system you want to restore\.

1. Choose the **Destination** tab and accept the default settings\.

1. Choose the **Schedule** tab, choose the repeat method that you want to use, and then choose **Submit**\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)