# Testing Your Setup by Using IBM Spectrum Protect<a name="backup-tsm"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using IBM Spectrum Protect \(formerly known as Tivoli Storage Manager\)\. In this topic, you can find basic documentation on how to configure the IBM Spectrum Protect Version 7\.x backup software for a tape gateway and perform backup and restore operations\. For detailed information about how to use IBM Spectrum Protect backup software, see the [IBM Spectrum Protect Administrator's Guide](https://www.ibm.com/support/knowledgecenter/SSTG2D_7.1.0/com.ibm.itsm.srv.doc/b_srv_admin_guide_windows.pdf)\.

**Note**  
The IBM Spectrum Protect backup software is only supported on the Microsoft Windows\.

## Setting Up IBM Spectrum Protect<a name="tsm-setup"></a>

After you have connected your virtual tape library \(VTL\) devices to your Windows client, you configure the IBM Spectrum Protect Version 7\.x software to recognize your VTL devices\. For information about how to connect VTL devices to your client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

**To set up IBM Spectrum Protect**

1. Get a licensed copy of the IBM Spectrum Protect version 7\.1\.9 software from IBM\.

1.  Install the IBM Spectrum Protect software on your on\-premises environment or in\-cloud Amazon EC2 instance\. For installation instruction, see the [IBM Spectrum Protect Administrator's Guide](https://www.ibm.com/support/knowledgecenter/SSTG2D_7.1.0/com.ibm.itsm.srv.doc/b_srv_admin_guide_windows.pdf)\. For additional installation guidance, see the [IBM Spectrum Protect Tape Solution Guide](https://www.ibm.com/developerworks/community/files/basic/anonymous/api/library/4a5b0e43-b165-49c7-ae33-b1480e6840cb/document/65902d39-4943-46cb-9822-50fb6ecfd2ea/media)\.

## Configuring IBM Spectrum Protect to Work with VTL Devices<a name="tsm-configure"></a>

Next, configure IBM Spectrum Protect to work with your VTL devices\. For instructions, see [Tape Device Driver\-W12 6266 for Windows 2012](https://datacentersupport.lenovo.com/us/en/products/storage/tape-and-backup/ts2240/6160/downloads/ds502099)\.

**To configure IBM Spectrum Protect**

1. Get the correct driver package for your media changer\. For the tape\-device driver, IBM Spectrum Protect requires version W12 6266 for Windows 2012\. For instructions on how to get the drivers, see [Tape Device Driver\-W12 6266 for Windows 2012](https://datacentersupport.lenovo.com/us/en/products/storage/tape-and-backup/ts2240/6160/downloads/ds502099) on the Lenovo website\.
**Note**  
Make sure that you install the "non\-exclusive" set of drivers\.

1. On your computer, open **Computer Management**, expand** Media Changer devices**, and verify that the media changer type is listed as **IBM 3584 Tape Library**\.

1. Ensure that the barcode for any tape in the virtual tape library is eight characters or less\. If you try to assign your tape a barcode that is longer than eight characters, you get this error message: `"Tape barcode is too long for media changer"`\.

1. Ensure that all your tape drives and media changer appear in IBM Spectrum Protect\. To do so, use the following command: `\Tivoli\TSM\server>tsmdlst.exe`

## Writing Data to a Tape in IBM Spectrum Protect<a name="tsm-write-data-to-tape"></a>

You write data to a tape gateway virtual tape by using the same procedure and backup policies that you do with physical tapes\. Create the necessary configuration for backup and restore jobs\. For detailed information, see the [IBM Spectrum Protect Administrator's Guide](https://www.ibm.com/support/knowledgecenter/SSTG2D_7.1.0/com.ibm.itsm.srv.doc/b_srv_admin_guide_windows.pdf)\.

## Restoring Data from a Tape Archived in IBM Spectrum Protect<a name="tsm-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape from archive to a tape gateway\. For instructions, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Restore the data by using the IBM Spectrum Protect backup software\. You do this by creating a recovery point, as you do when restoring data from physical tapes\. For instructions, see the [IBM Spectrum Protect Administrator's Guide](https://www.ibm.com/support/knowledgecenter/SSTG2D_7.1.0/com.ibm.itsm.srv.doc/b_srv_admin_guide_windows.pdf)\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)