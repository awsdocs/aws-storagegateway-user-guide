# Managing Your Tape Gateway<a name="managing-gateway-vtl"></a>

Following, you can find information about how to manage your tape gateway resources\.


+ [Adding Virtual Tapes](#creating-virtual-tapes-vtl)
+ [Retrieving Archived Tapes](#retrieving-archived-tapes-vtl)
+ [Viewing Tape Usage](#tape-usage)
+ [Deleting Tapes](#deleting-tapes-vtl)
+ [Disabling Your Tape Gateway](#disabling-gateway-vtl)
+ [Understanding Tape Status](#understand-tapes-status)

## Adding Virtual Tapes<a name="creating-virtual-tapes-vtl"></a>

You can add tapes in your tape gateway when you need them\. For information about how to create tape, see [Creating Tapes](GettingStartedCreateTapes.md)\. After your tape is created,information about your tape is displayed in the columns and in the **Details** tab of your tape library\. For information about tape gateway tape limits, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\.

## Retrieving Archived Tapes<a name="retrieving-archived-tapes-vtl"></a>

To access data stored on an archived virtual tape, you must first retrieve the archived virtual tape you want to your tape gateway\. Tape gateway provides one VTL per gateway\. You can restore a tape to a tape gateway\.

If you have multiple tape gateways in a region, you can retrieve a tape to only one gateway\.

The retrieved tape is write\-protected; you can only read the data on the tape\.

**Important**  
It takes up to 3 to 5 hours for the tape to be available in your tape gateway\.

**Note**  
There is a charge for retrieving tapes from archive\. For detailed pricing information, see [AWS Storage Gateway Pricing](http://aws.amazon.com/storagegateway/pricing/)\.

**To retrieve an archived tape to your gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**\. You can search for all tapes that are archived to displays all virtual tapes that have been archived by all your gateways\. 

1. Choose the virtual tape you want to retrieve, and choose **Retrieve Tape** from the **Actions** menu\. 
**Note**  
The status of the virtual tape you want to retrieve must be ARCHIVED\.

1. In the **Retrieve tape** dialog box, for **Barcode**, verify that the barcode identifies the virtual tape you want to retrieve\.

1. For **Gateway**, choose the gateway you want to retrieve the archived tape to, and then choose **Retrieve tape**\.

The status of the tape changes from ARCHIVED to RETRIEVING\. At this point, your data is being moved from the virtual tape shelf \(backed by Amazon Glacier\) to the virtual tape library \(backed by Amazon S3\)\. After all the data is moved, the status of the virtual tape in the archive changes to RETRIEVED\. 

**Note**  
Retrieved virtual tapes are read\-only\.

## Viewing Tape Usage<a name="tape-usage"></a>

When you write data to a tape, you can view the amount of data stored on the tape in the AWS Storage Gateway Management Console\. The **Details** tab for each tape shows the tape usage information\.

**To view the amount of data stored on a tape**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes** and select the tape you are interested in\.

1. Choose the **Details** tab\.

1. The following fields provide information about the tape:

   + **Size:** The total capacity of the selected tape\.

   + **Used:** The size of data written to the tape by your backup application\. 
**Note**  
This value is not available for tapes created prior to May,13 2015\.

## Deleting Tapes<a name="deleting-tapes-vtl"></a>

You can delete virtual tapes from your tape gateway by using the AWS Storage Gateway console\.

**Note**  
If the tape you want to delete from your tape gateway has a status of RETRIEVED, you must first eject the tape using your backup application before deleting the tape\. For instructions on how to eject a tape using the Symantec NetBackup software, see [Archiving the Tape](backup_netbackup-vtl.md#GettingStarted-archiving-tapes-vtl)\. After the tape is ejected, the tape status changes back to ARCHIVED\. You can then delete the tape\. 

Make copies of your data before you delete your tapes\. Once you delete a tape you can't get it back\.

**To delete a virtual tape**
**Warning**  
This procedure will permanently delete the selected virtual tape\.

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**\. 

1. Choose the virtual tape you want to delete\. 

1. On the **Actions** menu, choose **Delete tape** to open the confirmation box\.

1.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/delete-tape.png)

1. Make sure the tape listed is the tape you intend to delete, select the confirmation check box, and then choose **Delete**\.

   After the tape is deleted, it disappears from the tape gateway\.

## Disabling Your Tape Gateway<a name="disabling-gateway-vtl"></a>

You disable a tape gateway if the tape gateway has failed and you want to recover the tapes from the failed gateway to another gateway\. To recover the tapes, you must first disable the failed gateway\. Disabling a tape gateway locks down the virtual tapes in that gateway\. That is, any data that you might write to these tapes after disabling the gateway will not be sent to AWS\. You can only disable a gateway on the Storage Gateway console if the gateway is no longer connected to AWS\. If the gateway is connected to AWS, you won't be able to disable the tape gateway\. You disable a tape gateway as part of data recovery\. For more information about recovering tapes, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\. 

**To disable your gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways** and choose the failed gateway\.

1. Choose the **Details** tab for the gateway to display the disable gateway message\.

1. Choose **Create recovery tapes**\.

1. Choose **Disable gateway**\.

## Understanding Tape Status<a name="understand-tapes-status"></a>

Each tape has an associated status that tells you at a glance what the health of the tape is\. Most of the time, the status indicates that the tape is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem with the tape that might or might not require action on your part\. You can find information following to help you decide when you need to take action\.


+ [Tape Status Information in a VTL](#tape-status)
+ [Tape Status Information in the Archive](#determine-tape-status-vts)

### Tape Status Information in a VTL<a name="tape-status"></a>

A tape's status must be AVAILABLE for you to read or write to the tape\. The following table lists and describes possible status values\.


| Status | Description | Tape Data Is Stored In | 
| --- | --- | --- | 
| CREATING | The virtual tape is being created\. The tape cannot be loaded into a tape drive, because it is being created\. | — | 
| AVAILABLE | The virtual tape is created and ready to be loaded into a tape drive\. | Amazon S3 | 
| IN TRANSIT TO VTS | The virtual tape has been ejected and is being uploaded for archive\. At this point, your tape gateway is uploading data to AWS\. If the amount of data being uploaded is small, this status might not be seen\. When the upload is completed, the status changes to ARCHIVING\.  |  Amazon S3  | 
| ARCHIVING | The virtual tape is being moved by tape gateway to the archive, which is backed by Amazon Glacier\. This process happens after the data upload to AWS is completed\.  | Data is being moved from Amazon S3 to Amazon Glacier | 
| DELETING | The virtual tape is being deleted\. | Data is being deleted from Amazon S3 | 
| DELETED | The virtual tape has been successfully deleted\. | — | 
| RETRIEVING | The virtual tape is being retrieved from the archive to your tape gateway\.  The virtual tape can be retrieved only to a tape gateway\.   | Data is being moved from Amazon Glacier to Amazon S3 | 
| RETRIEVED | The virtual tape is retrieved from the archive\. The retrieved tape is write\-protected\. | Amazon S3 | 
| RECOVERED |  The virtual tape is recovered and is read\-only\.  When your tape gateway is not accessible for any reason, you can recover virtual tapes associated with that tape gateway to another tape gateway\. You must disable the inaccessible tape gateway before you can recover the virtual tapes\.   | Amazon S3 | 
| IRRECOVERABLE | The virtual tape cannot be read from or written to\. This status indicates an error in your tape gateway\. | Amazon S3 | 

### Tape Status Information in the Archive<a name="determine-tape-status-vts"></a>

**To determine the status of a virtual tape**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**\. 

1. In the **Status** column of the tape library grid, check the status of the tape\.

   The tape status also appears in the **Details** tab of each virtual tape\.

The following table lists and describes the possible status values\.


| Status | Description | 
| --- | --- | 
| ARCHIVED | The virtual tape has been ejected and is uploaded to the archive\. | 
| RETRIEVING | The virtual tape is being retrieved from the archive\.  The virtual tape can be retrieved only to a tape gateway\.    | 
| RETRIEVED | The virtual tape has been retrieved from the archive\. The retrieved tape is read\-only\. | 

For additional information about how to work with tapes and VTL devices, see [Working With Tapes](managing-virtual-tapes-vtl.md)\.