# Testing Your Setup by Using Veeam Backup & Replication<a name="backup-Veeam"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using Veeam Backup & Replication V7, V8, or V9 Update 2 or later\. In this topic, you can find basic documentation on how to configure the Veeam Backup & Replication software for a tape gateway and perform a backup and restore operation\. For detailed information about how to use the Veeam software, see the [Veeam Backup & Replication documentation](http://helpcenter.veeam.com/backup/70/hyperv/working_with_tape_media.html) in the Veeam Help Center\. For more information about compatible backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.

**Topics**
+ [Configuring Veeam to Work with VTL Devices](#veeam-configure-software)
+ [Importing a Tape into Veeam](#veeam-Import-tapes)
+ [Backing Up Data to a Tape in Veeam](#veeam-write-data-to-tape)
+ [Archiving a Tape by Using Veeam](#veeam-archive-tape)
+ [Restoring Data from a Tape Archived in Veeam](#veeam-restore-tape)

## Configuring Veeam to Work with VTL Devices<a name="veeam-configure-software"></a>

After you have connected your virtual tape library \(VTL\) devices to the Windows client, you configure Veeam Backup & Replication to recognize your devices\. For information about how to connect VTL devices to the Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

### Updating VTL Device Drivers<a name="veeam-update-driver"></a>

By default, the Veeam V7 and V8 backup application does not recognize tape gateway devices\. To configure the software to work with tape gateway devices, you update the device drivers for the VTL devices to expose them to the Veeam software and then discover the VTL devices\. In Device Manager, update the driver for the medium changer\. For instructions, see [Updating the Device Driver for Your Medium Changer](resource_vtl-devices.md#update-vtl-device-driver)\.

### Discovering VTL Devices<a name="veeam-dicorver-tapes"></a>

For the Veeam 9 backup application, you must use native SCSI commands instead of a Windows driver to discover your tape library if your media changer is unknown\. For detailed instructions, see [Working with Tape Libraries](https://helpcenter.veeam.com/backup/vsphere/managing_library.html)\.

**To discover VTL devices**

1. In the Veeam software, choose **Backup Infrastructure**\. When the tape gateway is connected, virtual tapes are listed in the **Backup Infrastructure** tab\.
**Note**  
Depending on the version of the Veeam Backup & Replication you are using, the user interface might differ somewhat from that shown in the screenshots in this documentation\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/veeam2.png)

1. Expand the **Tape** tree to see your tape drives and medium changer\.

1. Expand the medium changer tree\. If your tape drives are mapped to the medium changer, the drives appear under **Drives**\. Otherwise, your tape library and tape drives appear as separate devices\. 

   If the drives are not mapped automatically, follow the [instructions on the Veeam website](http://www.veeam.com/kb1842) to map the drives\. 

## Importing a Tape into Veeam<a name="veeam-Import-tapes"></a>

You are now ready to import tapes from your tape gateway into the Veeam backup application library\.

**To import a tape into the Veeam library**

1. Open the context \(right–click\) menu for the medium changer, and choose **Import** to import the tapes to the I/E slots\.

1. Open the context \(right–click\) menu for the medium charger, and choose **Inventory Library** to identify unrecognized tapes\. When you load a new virtual tape into a tape drive for the first time, the tape is not recognized by the Veeam backup application\. To identify the unrecognized tape, you inventory the tapes in the tape library\.

## Backing Up Data to a Tape in Veeam<a name="veeam-write-data-to-tape"></a>

Backing data to a tape is a two\-step process: 

1. You create a media pool and add the tape to the media pool\.

1. You write data to the tape\.

You create a media pool and write data to a virtual tape by using the same procedures you do with physical tapes\. For detailed information about how to back up data, see the [Veeam documentation](http://helpcenter.veeam.com/backup/70/hyperv/index.html?getting_started_with_tapes.html) in the Veeam Help Center\.

## Archiving a Tape by Using Veeam<a name="veeam-archive-tape"></a>

When you archive a tape, tape gateway moves the tape from the Veeam tape library to the offline storage\. You begin tape archival by ejecting from the tape drive to the storage slot and then exporting the tape from the slot to the archive by using your backup application—that is, the Veeam software\.

**To archive a tape in the Veeam library**

1. Choose **Backup Infrastructure**, and choose the media pool that contains the tape you want to archive\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/veeam-archive-tape2.png)

1. Open the context \(right–click\) menu for the tape that you want to archive, and then choose **Eject Tape**\.

1. For **Ejecting tape**, choose **Close**\. The location of the tape changes from a tape drive to a slot\.

1. Open the context \(right–click\) menu for the tape again, and then choose **Export**\. The status of the tape changes from **Tape drive **to **Offline**\.

1. For **Exporting tape**, choose **Close**\. The location of the tape changes from **Slot** to **Offline**\.

1. On the AWS Storage Gateway console, choose your gateway, and then choose **VTL Tape Cartridges** and verify the status of the virtual tape you are archiving\. 

   The archiving process can take some time to complete\. The initial status of the tape appears as **IN TRANSIT TO VTS**\. When archiving starts, the status changes to **ARCHIVING**\. When archiving is completed, the tape is no longer listed in the VTL\.

## Restoring Data from a Tape Archived in Veeam<a name="veeam-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape from archive to a tape gateway\. For instructions, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use the Veeam software to restore the data\. You do this by creating a restoring a folder file, as you do when restoring data from physical tapes\. For instructions, see [Restoring Data from Tape](http://helpcenter.veeam.com/backup/70/hyperv/restoring_data_from_tape.html) in the Veeam Help Center\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)