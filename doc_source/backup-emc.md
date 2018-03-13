# Testing Your Setup by Using Dell EMC NetWorker<a name="backup-emc"></a>

You can back up your data to virtual tapes, archive the tapes and manage your virtual tape library \(VTL\) devices by using Dell EMC NetWorker version 8\.x or 9\.x\. In this topic, you can find basic documentation on how to configure the Dell EMC NetWorker software to work with a tape gateway and perform a backup, including how to configure storage devices, write data to a tape, archive a tape and restore data from a tape\. This documentation uses the Dell NetWorker V9\.x as an example\.

For detailed information about how to install and use the Dell EMC NetWorker software, see the *[EMC NetWorker Administration Guide](http://www.emc.com/collateral/TechnicalDocument/docu53903.pdf)*\.

For more information about compatible backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.


+ [Configuring Dell EMC NetWorker to Work with VTL Devices](#emc-configure-software)
+ [Enabling Import of WORM Tapes into Dell EMC NetWorker](#emc-import-tapes)
+ [Backing Up Data to a Tape in Dell EMC NetWorker](#emc-write-data-to-tape)
+ [Archiving a Tape in Dell EMC NetWorker](#emc-archive-tape)
+ [Restoring Data from an Archived Tape in Dell EMC NetWorker](#emc-restore-tape)

## Configuring Dell EMC NetWorker to Work with VTL Devices<a name="emc-configure-software"></a>

After you have connected your virtual tape library \(VTL\) devices to your Microsoft Windows client, you configure Dell EMC NetWorker to recognize your devices\. For information about how to connect VTL devices to the Windows client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

Dell EMC NetWorker doesn't automatically recognize tape gateway devices\. To expose your VTL devices to the NetWorker software and get the software to discover them, you manually configure the software\. Following, we assume that you have correctly installed the Dell EMC NetWorker software and that you are familiar with the Dell EMC NetWorker Management Console\. For more information about the Dell EMC NetWorker Management Console, see the NetWorker Management Console interface section of the *[ EMC NetWorker Administration Guide](http://www.emc.com/collateral/TechnicalDocument/docu53903.pdf)\.* 

The following screenshot shows the Dell EMC NetWorker V9\.x Management Console\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/emc-console.png)

**To configure the Dell EMC NetWorker software for VTL devices**

1. Start the Dell EMC NetWorker Management Console application, choose **Enterprise** from the menu, and then choose **localhost** from the left pane\.

1. Open the context \(right\-click\) menu for **localhost**, and then choose **Launch Application**\.

1. Choose the **Devices** tab, open the context \(right\-click\) menu for **Libraries**, and then choose **Scan for Devices**\.

1. In the Scan for Devices wizard, choose **Start Scan**, and then choose **OK** from the dialog box that appears\.

1. Expand the **Libraries** folder tree to see all your libraries\. This process might take a few seconds to load the devices into the library\.

1. Open the context \(right\-click\) menu for your library, and then choose **Configure All Libraries**\.

1. In the **Provide General Configuration Information** box, choose the configuration settings you want, and then choose **Next**\. 

1. In the **Select Target Storage Nodes** box, verify that a storage node is selected, and then choose **Start Configuration**\. The selected storage node will have **Configure** selected\. 

1. In the Start Configuration wizard, choose **Finish**\.

1. Choose your library to see your tapes in the left pane and the corresponding empty volume slots list in the right pane\. In this screenshot, the **AWS@3\.0\.0** library is selected\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/emc-media.png)

1. In the volume list, select the volumes you want to enable \(selected volumes are highlighted\), open the context \(right\-click\) menu for the selected volumes, and then choose **Deposit**\. This action moves the tape from the I/E slot into the volume slot\.

1. In the dialog box that appears, choose **Yes**, and then in the **Load the Cartridges into** dialog box, choose **Yes**\. 

1. If you don't have any more tapes to deposit, choose **No** or **Ignore**\. Otherwise, choose **Yes** to deposit additional tapes\.

## Enabling Import of WORM Tapes into Dell EMC NetWorker<a name="emc-import-tapes"></a>

You are now ready to import tapes from your tape gateway into the Dell EMC NetWorker library\.

The virtual tapes are write once read many \(WORM\) tapes, but Dell EMC NetWorker expects non\-WORM tapes\. For Dell EMC NetWorker to work with your virtual tapes, you must enable import of tapes into non\-WORM media pools\. 

**To enable import of WORM tapes into non\-WORM media pools**

1. On NetWorker Console, choose **Media**, open the context \(right\-click\) menu for **localhost**, and then choose **Properties**\.

1.  In the **NetWorker Sever Properties** window, choose the **Configuration** tab\.

1.  In the **Worm tape handling** section, clear the **WORM tapes only in WORM pools** box, and then choose **OK**\.

## Backing Up Data to a Tape in Dell EMC NetWorker<a name="emc-write-data-to-tape"></a>

Backing up data to a tape is a two\-step process\. 

1. Label the tapes you want to back up your data to, create the target media pool, and add the tapes to the pool\.

   You create a media pool and write data to a virtual tape by using the same procedures you do with physical tapes\. For detailed information, see the Backing Up Data section of the [Dell EMC NetWorker Administration Guide](http://www.emc.com/collateral/TechnicalDocument/docu53903.pdf)\.

1. Write data to the tape\. You back up data by using the Dell EMC NetWorker User application instead of the Dell EMC NetWorker Management Console\. The Dell EMC NetWorker User application installs as part of the NetWorker installation\.

**Note**  
You use the Dell EMC NetWorker User application to perform backups, but you view the status of your backup and restore jobs in the EMC Management Console\. To view status, choose the **Devices** menu and view the status in the **Log** window\.

## Archiving a Tape in Dell EMC NetWorker<a name="emc-archive-tape"></a>

When you archive a tape, tape gateway moves the tape from the Dell EMC NetWorker tape library to the offline storage\. You begin tape archival by ejecting a tape from the tape drive to the storage slot\. You then withdraw the tape from the slot to the archive by using your backup application—that is, the Dell EMC NetWorker software\.

**To archive a tape by using Dell EMC NetWorker**

1. On the **Devices** tab in the NetWorker Administration window, choose **localhost** or your EMC server, and then choose **Libraries**\.

1. Choose the library you imported from your virtual tape library\.

1. From the list of tapes that you have written data to, open the context \(right\-click\) menu for the tape you want to archive, and then choose **Eject/Withdraw**\.

1. In the confirmation box that appears, choose **OK**\.

The archiving process can take some time to complete\. The initial status of the tape is shown as **IN TRANSIT TO VTS**\. When archiving starts, the status changes to **ARCHIVING**\. When archiving is completed, the tape is no longer listed in the VTL\.

## Restoring Data from an Archived Tape in Dell EMC NetWorker<a name="emc-restore-tape"></a>

Restoring your archived data is a two\-step process:

1. Retrieve the archived tape a tape gateway\. For instructions, see [Retrieving Archived Tapes ](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Use the Dell EMC NetWorker software to restore the data\. You do this by creating a restoring a folder file, as you do when restoring data from physical tapes\. For instructions, see the [ Using the NetWorker User program](http://www.emc.com/collateral/TechnicalDocument/docu53903.pdf) section of the *EMC NetWorker Administration Guide\.*

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)