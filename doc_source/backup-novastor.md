# Testing Your Setup by Using NovaStor DataCenter/Network<a name="backup-novastor"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using NovaStor DataCenter/Network version 6\.4 or 7\.1\. In this topic, you can find basic documentation on how to configure the NovaStor DataCenter/Network version 7\.1 backup application for a tape gateway and perform backup and restore operations\. For detailed information about how to use NovaStor DataCenter/Network version 7\.1, see [Documentation NovaStor DataCenter/Network](http://www.novastor.com/help-html/dc/en/Introduction.html)\.

## Setting Up NovaStor DataCenter/Network<a name="setting-up"></a>

After you have connected your virtual tape library \(VTL\) devices to your Microsoft Windows client, you configure the NovaStor software to recognize your devices\. For information about how to connect VTL devices to your Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

NovaStor DataCenter/Network requires drivers from the driver manufacturers\. You can use the Windows drivers, but you must first deactivate other backup applications\.

## Configuring NovaStor DataCenter/Network to Work with VTL Devices<a name="configuring-novastor"></a>

When configuring your VTL devices to work with NovaStor DataCenter/Network version 6\.4 or 7\.1, you might see an error message that reads `External Program did not exit correctly`\. This issue requires a workaround, which you need to perform before you continue\. 

You can prevent the issue by creating the workaround before you start configuring your VTL devices\. For information about how to create the workaround, see [Resolving an "External Program Did Not Exit Correctly" Error](#novastor-workaround)\.

**To configure NovaStor DataCenter/Network to work with VTL devices**

1. In the NovaStor DataCenter/Network Admin console, choose **Media Management**, and then choose **Storage Management**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/novastor1.png)

1. In the **Storage Targets** menu, open the context menu \(right\-click\) for **Media Management Servers**, choose **New**, and choose **OK** to create and prepopulate a **storage** node\. 

   If you see an error message that says `External Program did not exit correctly`, resolve the issue before you continue\. This issue requires a workaround\. For information about how to resolve this issue, see [Resolving an "External Program Did Not Exit Correctly" Error](#novastor-workaround) in the NovaStor documentation\.
**Important**  
This error occurs because the element assignment range from AWS Storage Gateway for storage drives and tape drives exceeds the number that NovaStor DataCenter/Network allows\.

1. Open the context \(right\-click\) menu for the **storage** node that was created, and choose **New Library**\.

1. Choose the library server from the list\. The library list is automatically populated\.

1. Name the library and choose **OK**\.

1. Choose the library to display all the properties of the Storage Gateway virtual tape library\.

1. In the **Storage Targets** menu, expand **Backup Servers**, open the context \(right\-click\) menu for the server, and choose **Attach Library**\.

1. In the **Attach Library** dialog box that appears, choose the **LTO5** media type, and then choose **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/LTO5.png)

1. Expand **Backup Servers** to see the Storage Gateway virtual tape library and the library partition that shows all the mounted tape drives\.

## Creating a Tape Pool<a name="create-tape-pool"></a>

A tape pool is dynamically created in the NovaStor DataCenter/Network software and so doesn't contain a fixed number of media\. A tape pool that needs a tape gets it from its scratch pool\. A *scratch pool* is a reservoir of tapes that are freely available for one or more tape pools to use\. A tape pool returns to the scratch pool any media that have exceeded their retention times and that are no longer needed\.

Creating a tape pool is a three\-step task:

1. You create a scratch pool\.

1. You assign tapes to the scratch pool\.

1. You create a tape pool\.

**To create a scratch pool**

1. In the left navigation menu, choose the **Scratch Pools** tab\. 

1. Open the context \(right\-click\) menu for **Scratch Pools**, and choose **Create Scratch Pool**\. 

1. In the **Scratch Pools** dialog box, name your scratch pool, and then choose your media type\.

1. Choose **Label Volume**, and create a low water mark for the scratch pool\. When the scratch pool is emptied down to the low water mark, a warning appears\.

1. In the warning dialog box that appears, choose **OK** to create the scratch pool\.

**To assign tapes to a scratch pool**

1. In the left navigation menu, choose **Tape Library Management**\.

1. Choose the **Library** tab to see your library's inventory\.

1. Choose the tapes that you want to assign to the scratch pool\. Make sure that the tapes are set to the correct media type\.

1. Open the context \(right\-click\) menu for the library and choose **Add to Scratch Pool**\. 

You now have a filled scratch pool that you can use for tape pools\.

**To create a tape pool**

1. From the left navigation menu, choose **Tape Library Management**\.

1. Open the context \(right\-click\) menu for the **Media Pools** tab and choose **Create Media Pool**\.

1. Name the media pool and choose **Backup Server**\.

1. Choose a library partition for the media pool\.

1. Choose the scratch pool that you want the pool to get the tapes from\.

1. For **Schedule**, choose **Not Scheduled**\.

## Configuring Media Import and Export to Archive Tapes<a name="configure-media-import"></a>

NovaStor DataCenter/Network can use import/export slots if they are part of the media changer\. 

For an export, NovaStor DataCenter/Network must know which tapes are going to be physically taken out of the library\. 

For an import, NovaStor DataCenter/Network recognizes tape media that are exported in the tape library and offers to import them all, either from a data slot or an export slot\. Your tape gateway archives tapes in the virtual tape shelf \(VTS\), which is backed by Amazon Glacier\. The VTS is referred to as offsite location in NovaStor DataCenter/Network\.

**To configure media import and export**

1. Navigate to **Tape Library Management**, choose a server for **Media Management Server**, and then choose **Library**\.

1. Choose the **Off\-site Locations** tab\.

1. Open the context \(right\-click\) menu for the white area, and choose **Add** to open a new panel\.

1. In the panel, type **Glacier** and add an optional description in the text box\.

## Backing Up Data to Tape<a name="novastor-backup-data"></a>

You create a backup job and write data to a virtual tape by using the same procedures that you do with physical tapes\. For detailed information about how to back up data using the NovaStor software, see [Start Backup Job](http://www.novastor.com/help-html/dc/en/StartBackupJob.html) in the NovaStor documentation\.

## Archiving a Tape<a name="novastor-archive-tape"></a>

When you archive a tape, a tape gateway ejects the tape from the tape drive to the storage slot\. It then exports the tape from the slot to the archive by using your backup application—that is, NovaStor DataCenter/Network\.

**To archive a tape**

1. In the left navigation menu, choose **Tape Library Management**\.

1. Choose the **Library** tab to see the library's inventory\.

1. Highlight the tapes you want to archive, open the context \(right\-click\) menu for the tapes, and choose **Mail Slot Export to Glacier**\.

The archiving process can take some time to complete\. The initial status of the tape appears as **IN TRANSIT TO VTS**\. When archiving starts, the status changes to **ARCHIVING**\. When archiving is completed, the tape is no longer listed in the VTL\.

In NovaStor DataCenter/Network, verify that the tape is no longer in the storage slot\.

In the navigation pane of the Storage Gateway console, choose **Tapes**\. Verify that your archived tape's status is **ARCHIVED**\. 

## Restoring Data from an Archived and Retrieved Tape<a name="novastor-retrieve-from-archive"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape from archive to a tape gateway\. For instructions, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use the NovaStor DataCenter/Network software to restore the data\. You do this by refreshing the mail slot and moving each tape you want to retrieve into an empty slot, as you do when restoring data from physical tapes\. For instructions, see [Restore the Example](http://www.novastor.com/help-html/dc/en/RestoretheExample.html) in the NovaStor documentation\.

## Writing Several Backup Jobs to a Tape Drive at the Same Time<a name="novastor-muliplexing"></a>

In the NovaStor software, you can write several jobs to a tape drive at the same time using the multiplexing feature\. This feature is available when a multiplexer is available for a media pool\. For information about how to use multiplexing, see [Define Backup Destination and Schedule](http://www.novastor.com/help-html/dc/en/DefineBackupDestinationandSchedu.html) in the NovaStor documentation\.

## Resolving an "External Program Did Not Exit Correctly" Error<a name="novastor-workaround"></a>

When configuring your VTL devices to work with NovaStor DataCenter/Network version 6\.4 or 7\.1, you might see an error message that reads `External Program did not exit correctly`\. This error occurs because the element assignment range from AWS Storage Gateway for storage drives and tape drives exceeds the number that NovaStor DataCenter/Network allows\. 

Storage Gateway returns 3200 storage and import/export slots, which is more than the 2400 limit that NovaStor DataCenter/Network allows\. To resolve this issue, you add a configuration file that enables the NovaStor software to limit the number of storage and import/export slots and preconfigures the element assignment range\. 

**To apply the workaround for an "external program did not exit correctly" error**

1. Navigate to the Tape folder on your computer where you installed the NovaStor software\.

1. In the Tape folder, create a text file and name it `hijacc.ini`\.

1. Copy the following content, paste it into `hijacc.ini` file, and save the file\.

   ```
   port:12001
   san:no
   define: A3B0S0L0
   *DRIVES: 10
   *FIRST_DRIVE: 10000
   *SLOTS: 200
   *FIRST_SLOT: 20000
   *HANDLERS: 1
   *FIRST_HANDLER: 0
   *IMP-EXPS: 30
   *FIRST_IMP-EXP: 30000
   ```

1. Add and attach the library to the media management server\.

1. Move a tape from the import/export slot into the library by using the following command as shown the screenshots below\. In the command, replace VTL with the name of your library\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/novastor-workaround-command1.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/novastor-workaround-command2.png)

1. Attach the library to the backup server\.

1. In the NovaStor software, import all the tapes from import/export slots into the library\.