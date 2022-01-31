--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Testing Your Setup by Using IBM Spectrum Protect<a name="backup-tsm"></a>

You can back up your data to virtual tapes, archive the tapes, and manage your virtual tape library \(VTL\) devices by using IBM Spectrum Protect with AWS Storage Gateway\. \(IBM Spectrum Protect was formerly known as Tivoli Storage Manager\.\) 

This topic contains basic information about how to configure the IBM Spectrum Protect version 8\.1\.10 backup software for a Tape Gateway\. It also includes basic information about performing backup and restore operations with IBM Spectrum Protect\. For more information about how to administer IBM Spectrum Protect backup software, see IBM's [Overview of administration tasks](https://www.ibm.com/support/knowledgecenter/en/SSEQVQ_8.1.10/srv.admin/t_administer_solution.html) for IBM Spectrum Protect\.

The IBM Spectrum Protect backup software supports AWS Storage Gateway on the following operating systems\.
+ **Microsoft Windows Server**
+ **Red Hat Linux**
+ **SUSE Linux**

For information about IBM Spectrum Protect supported devices for Windows, see [IBM Spectrum Protect \(formerly Tivoli Storage Manager\) Supported Devices for AIX, HP\-UX, Solaris, and Windows](https://www.ibm.com/support/pages/node/716993)\.

For information about IBM Spectrum Protect supported devices for Linux, see [IBM Spectrum Protect \(formerly Tivoli Storage Manager\) Supported Devices for Linux](https://www.ibm.com/support/pages/node/716987)\.

**Topics**
+ [Setting Up IBM Spectrum Protect](#tsm-setup)
+ [Configuring IBM Spectrum Protect to Work with VTL Devices](#tsm-configure)
+ [Writing Data to a Tape in IBM Spectrum Protect](#tsm-write-data-to-tape)
+ [Restoring Data from a Tape Archived in IBM Spectrum Protect](#tsm-restore-tape)

## Setting Up IBM Spectrum Protect<a name="tsm-setup"></a>

After you connect your VTL devices to your client, you configure the IBM Spectrum Protect version 8\.1\.10 software to recognize them\. For more information about connecting VTL devices to your client, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL)\.

**To set up IBM Spectrum Protect**

1. Get a licensed copy of the IBM Spectrum Protect version 8\.1\.10 software from IBM\.

1.  Install the IBM Spectrum Protect software on your on\-premises environment or in\-cloud Amazon EC2 instance\. For more information, see IBM's [Installing and upgrading](https://www.ibm.com/support/knowledgecenter/en/SSEQVQ_8.1.10/srv.common/t_installing_upgrading.html) documentation for IBM Spectrum Protect\. 

   For more information about configuring IBM Spectrum Protect software, see [Configuring AWS Tape Gateway virtual tape libraries for an IBM Spectrum Protect server](https://www.ibm.com/support/pages/node/6326793)\.

## Configuring IBM Spectrum Protect to Work with VTL Devices<a name="tsm-configure"></a>

Next, configure IBM Spectrum Protect to work with your VTL devices\. You can configure IBM Spectrum Protect to work with VTL devices on Microsoft Windows Server, Red Hat Linux, or SUSE Linux\.

### Configuring IBM Spectrum Protect for Windows<a name="tsm-configure-windows"></a>

For complete instructions on how to configure IBM Spectrum Protect on Windows, see [Tape Device Driver\-W12 6266 for Windows 2012](https://datacentersupport.lenovo.com/us/en/products/storage/tape-and-backup/ts2240/6160/downloads/ds502099) on the Lenovo website\. Following is basic documentation on the process\.

**To configure IBM Spectrum Protect for Microsoft Windows**

1. Get the correct driver package for your media changer\. For the tape\-device driver, IBM Spectrum Protect requires version W12 6266 for Windows 2012\. For instructions on how to get the drivers, see [Tape Device Driver\-W12 6266 for Windows 2012](https://datacentersupport.lenovo.com/us/en/products/storage/tape-and-backup/ts2240/6160/downloads/ds502099) on the Lenovo website\.
**Note**  
Make sure that you install the "non\-exclusive" set of drivers\.

1. On your computer, open **Computer Management**, expand** Media Changer devices**, and verify that the media changer type is listed as **IBM 3584 Tape Library**\.

1. Ensure that the barcode for any tape in the virtual tape library is eight characters or less\. If you try to assign your tape a barcode that is longer than eight characters, you get this error message: `"Tape barcode is too long for media changer"`\.

1. Ensure that all your tape drives and media changer appear in IBM Spectrum Protect\. To do so, use the following command: `\Tivoli\TSM\server>tsmdlst.exe`

### Configure IBM Spectrum Protect for Linux<a name="tsm-configure-linux"></a>

Following is basic documentation on configuring IBM Spectrum to work with VTL devices on Linux\.

**To configure IBM Spectrum Protect for Linux**

1. Go to [IBM Fix Central](https://www.ibm.com/support/fixcentral/) on the IBM Support website, and choose **Select product**\.

1. For **Product Group**, choose **System Storage**\.

1. For **Select from System Storage**, choose **Tape systems**\.

1. For **Tape systems**, choose **Tape drivers and software**\.

1. For **Select from Tape drivers and software**, choose **Tape device drivers**\.

1. For **Platform**, choose your operating system and choose **Continue**\.

1. Choose the device driver version that you want to download\. Then follow the instructions on the **Fix Central** download page to download and configure IBM Spectrum Protect\.

1. Ensure that the barcode for any tape in the virtual tape library is eight characters or less\. If you try to assign your tape a barcode that is longer than eight characters, you get this error message: `"Tape barcode is too long for media changer"`\.

## Writing Data to a Tape in IBM Spectrum Protect<a name="tsm-write-data-to-tape"></a>

You write data to a Tape Gateway virtual tape by using the same procedure and backup policies that you do with physical tapes\. Create the necessary configuration for backup and restore jobs\. For more information about configuring IBM Spectrum Protect, see [Overview of administration tasks](https://www.ibm.com/support/knowledgecenter/en/SSEQVQ_8.1.10/srv.admin/t_administer_solution.html) for IBM Spectrum Protect\.

## Restoring Data from a Tape Archived in IBM Spectrum Protect<a name="tsm-restore-tape"></a>

Restoring your archived data is a two\-step process\.

**To restore data from an archived tape**

1. Retrieve the archived tape from archive to a Tape Gateway\. For instructions, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.

1. Restore the data by using the IBM Spectrum Protect backup software\. You do this by creating a recovery point, as you do when restoring data from physical tapes\. For more information about configuring IBM Spectrum Protect, see [Overview of administration tasks](https://www.ibm.com/support/knowledgecenter/en/SSEQVQ_8.1.10/srv.admin/t_administer_solution.html) for IBM Spectrum Protect\.

**Next Step**

[Cleaning Up Resources You Don't Need](GettingStartedWhatsNextStep3-vtl.md#cleanup-vtl)