--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Managing Your Tape Gateway<a name="managing-gateway-vtl"></a>

Following, you can find information about how to manage your Tape Gateway resources in AWS Storage Gateway\.

**Topics**
+ [Adding Virtual Tapes](#creating-virtual-tapes-vtl)
+ [Managing Automatic Tape Creation](#managing-automatic-tape-creation)
+ [Archiving Virtual Tapes](#archiving-tapes-vtl)
+ [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](#moving-tapes-vtl)
+ [Retrieving Archived Tapes](#retrieving-archived-tapes-vtl)
+ [Viewing Tape Usage](#tape-usage)
+ [Deleting Tapes](#deleting-tapes-vtl)
+ [Deleting Custom Tape Pools](#deleting-tape-pools-vtl)
+ [Disabling Your Tape Gateway](#disabling-gateway-vtl)
+ [Understanding Tape Status](#understand-tapes-status)

## Adding Virtual Tapes<a name="creating-virtual-tapes-vtl"></a>

You can add tapes in your Tape Gateway when you need them\. For information about how to create tapes, see [Creating Tapes](GettingStartedCreateTapes.md)\. 

After your tape is created, you can find information about it on the **Tapes** page\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\. For information about Tape Gateway tape quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

## Managing Automatic Tape Creation<a name="managing-automatic-tape-creation"></a>

The tape gateway automatically creates new virtual tapes to maintain the minimum number of available tapes that you configure\. It then makes these new tapes available for import by the backup application so that your backup jobs can run without interruption\. Automatic tape creation removes the need for custom scripting in addition to the manual process for creating new virtual tapes\. 

**To delete an automatic tape creation policy**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway for which you need to manage automatic tape creation\. 

1. In the **Actions** menu, choose **Configure tape auto\-create**\. 

1. To delete an automatic tape creation policy on a gateway, choose **Remove** to the right of the policy you want to delete\.

   To stop automatic tape creation on a gateway, delete all of the automatic tape creation policies for that gateway\.

   Choose **Save changes** to confirm deletion of tape auto\-create policies for the selected tape gateway\.
**Note**  
Deleting a tape auto\-creation policy from a gateway cannot be undone\.

**To change the automatic tape creation policies for a tape gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway for which you need to manage automatic tape creation\. 

1. In the **Actions** menu, choose **Configure tape auto\-create**, and change the settings on the page that appears\. 

1. For **Minimum number of tapes**, enter the minimum number of virtual tapes that should be available on the tape gateway at all times\. The valid range for this value is a minimum of 1 and a maximum of 10\. 

1. For **Capacity**, enter the size, in bytes of the virtual tape capacity\. The valid range for this value is a minimum of 100 GiB and a maximum of 5 TiB\. 

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tapes are stored when they are ejected by your backup software\. 
   + Choose **Glacier Pool** if you want to archive the tapes in the S3 Glacier Flexible Retrieval storage class\. When your backup software ejects the tapes, they are automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives, where you can retrieve a tape typically within 3\-5 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose **Deep Archive Pool** if you want to archive the tapes in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation, where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

   If you archive tapes in S3 Glacier Flexible Retrieval, you can move them to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](#moving-tapes-vtl)\.

1. You can find information about your tapes on the **Tapes** page\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   The status of available virtual tapes is initially set to **CREATING** when the tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](#managing-gateway-vtl)\.

   For more information about enabling automatic tape creation, see [Creating Tapes Automatically](GettingStartedCreateTapes.md#CreateTapesAutomatically)\.

## Archiving Virtual Tapes<a name="archiving-tapes-vtl"></a>

You can archive your tapes to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. When you create a tape, you choose the archive pool that you want to use to archive your tape\. 

You choose **Glacier Pool** if you want to archive the tape in S3 Glacier Flexible Retrieval\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives where the data is regularly retrieved and needed in minutes\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) 

You choose **Deep Archive Pool** if you want to archive the tape in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation at a very low cost\. Data in S3 Glacier Deep Archive is not retrieved often or is rarely retrieved\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)\.

**Note**  
Any tape created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects it\.

When your backup software ejects a tape, it is automatically archived in the pool that you chose when you created the tape\. The process for ejecting a tape varies depending on your backup software\. Some backup software requires that you export tapes after they are ejected before archiving can begin\. For information about supported backup software, see [Using Your Backup Software to Test Your Gateway Setup](GettingStartedTestGatewayVTL.md)\.

## Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class<a name="moving-tapes-vtl"></a>



Move your tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive for long\-term data retention and digital preservation at a very low cost\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation where the data is accessed once or twice a year\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)\.

**To move a tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive**

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Select the check boxes for the tapes you want to move to S3 Glacier Deep Archive\. You can see the pool that each tape is associated with in the **Pool** column\.

1. Choose **Assign to pool**\.

1. In the Assign tape to pool dialog box, verify the barcodes for the tapes you are moving and choose **Assign**\. 
**Note**  
If a tape has been ejected by the backup application and archived in S3 Glacier Deep Archive, you can't move it back to S3 Glacier Flexible Retrieval\. There's a charge for moving your tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive\. In addition, if you move tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive prior to 90 days, there is an early deletion fee for S3 Glacier Flexible Retrieval\.

1. After the tape is moved, you can see the updated status in the **Pool** column on the **Tapes** page\.

## Retrieving Archived Tapes<a name="retrieving-archived-tapes-vtl"></a>

To access data stored on an archived virtual tape, you must first retrieve the tape that you want to your Tape Gateway\. Your tape gateway provides one virtual tape library \(VTL\) for each gateway\.

If you have more than one Tape Gateway in an AWS Region, you can retrieve a tape to only one gateway\.

The retrieved tape is write\-protected; you can only read the data on the tape\.

 

**Important**  
If you archive a tape in S3 Glacier Flexible Retrieval, you can retrieve the tape typically within 3\-5 hours\. If you archive the tape in S3 Glacier Deep Archive, you can retrieve it typically within 12 hours\.

**Note**  
There is a charge for retrieving tapes from archive\. For detailed pricing information, see [Storage Gateway Pricing](http://aws.amazon.com/storagegateway/pricing/)\.

**To retrieve an archived tape to your gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Choose the virtual tape you want to retrieve from the **Virtual Tape Shelf** tab, and choose **Retrieve tape**\. 
**Note**  
The status of the virtual tape that you want to retrieve must be ARCHIVED\.

1. In the **Retrieve tape** dialog box, for **Barcode**, verify that the barcode identifies the virtual tape you want to retrieve\.

1. For **Gateway**, choose the gateway that you want to retrieve the archived tape to, and then choose **Retrieve tape**\.

The status of the tape changes from ARCHIVED to RETRIEVING\. At this point, your data is being moved from the virtual tape shelf \(backed by S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\) to the virtual tape library \(backed by Amazon S3\)\. After all the data is moved, the status of the virtual tape in the archive changes to RETRIEVED\. 

**Note**  
Retrieved virtual tapes are read\-only\.

## Viewing Tape Usage<a name="tape-usage"></a>

When you write data to a tape, you can view the amount of data stored on the tape in the Storage Gateway console\. The **Details** tab for each tape shows the tape usage information\.

**To view the amount of data stored on a tape**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Choose the tape you are interested in\.

1. The page that appears provides various details and information about the tape, including the following:
   + **Size:** The total capacity of the selected tape\.
   + **Used:** The size of data written to the tape by your backup application\. 
**Note**  
This value is not available for tapes created before May 13, 2015\.

## Deleting Tapes<a name="deleting-tapes-vtl"></a>

You can delete virtual tapes from your Tape Gateway by using the Storage Gateway console\.

**Note**  
If the tape you want to delete from your Tape Gateway has a status of RETRIEVED, you must first eject the tape using your backup application before deleting the tape\. For instructions on how to eject a tape using the Symantec NetBackup software, see [Archiving the Tape](backup_netbackup-vtl.md#GettingStarted-archiving-tapes-vtl)\. After the tape is ejected, the tape status changes back to ARCHIVED\. You can then delete the tape\. 

Make copies of your data before you delete your tapes\. After you delete a tape, you can't get it back\.

**To delete a virtual tape**
**Warning**  
This procedure permanently deletes the selected virtual tape\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Choose the virtual tape that you want to delete\. 

1. Choose **Delete tape**\. A confirmation box appears\.

1. Make sure that the tape listed is the tape you intend to delete, type *delete* in the confirmation field, and then choose **Delete**\.

After the tape is deleted, it disappears from the Tape Gateway\.

## Deleting Custom Tape Pools<a name="deleting-tape-pools-vtl"></a>

You can delete a custom tape pool only if there are no archived tapes in the pool, and there are no automatic tape creation policies attached to the pool\.

**To delete your custom tape pool**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Pools** to see the available pools\.

1. Choose the custom tape pools that you want to delete\.

   If the **Tape Count** for the tape pools that you want to delete is **0**, and if there are no automatic tape creation policies that reference the custom tape pool, you can delete the pools\.

1. Choose **Delete**\.

1. The **Confirm deletion** dialog box displays the **Name** and **Pool ID** of the pools to be deleted\. To delete the pools, enter **delete** in the confirmation field, then choose **Delete**\. 
**Warning**  
This procedure permanently deletes the selected custom tape pools and can't be undone\.

   After the custom tape pools are deleted, they disappear from the tape library\.

## Disabling Your Tape Gateway<a name="disabling-gateway-vtl"></a>

You disable a Tape Gateway if the Tape Gateway has failed and you want to recover the tapes from the failed gateway to another gateway\. 

To recover the tapes, you must first disable the failed gateway\. Disabling a Tape Gateway locks down the virtual tapes in that gateway\. That is, any data that you might write to these tapes after disabling the gateway isn't sent to AWS\. You can only disable a gateway on the Storage Gateway console if the gateway is no longer connected to AWS\. If the gateway is connected to AWS, you can't disable the tape gateway\. 

You disable a tape gateway as part of data recovery\. For more information about recovering tapes, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\. 

**To disable your gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the failed gateway\.

1. Choose the **Details** tab for the gateway to display the disable gateway message\.

1. Choose **Create recovery tapes**\.

1. Choose **Disable gateway**\.

## Understanding Tape Status<a name="understand-tapes-status"></a>

Each tape has an associated status that tells you at a glance what the health of the tape is\. Most of the time, the status indicates that the tape is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem with the tape that might require action on your part\. You can find information following to help you decide when you need to act\.

**Topics**
+ [Understanding Tape Status Information in a VTL](#tape-status)
+ [Determining Tape Status in an Archive](#determine-tape-status-vts)

### Understanding Tape Status Information in a VTL<a name="tape-status"></a>

A tape's status must be AVAILABLE for you to read or write to the tape\. The following table lists and describes possible status values\.


| Status | Description | Tape Data Is Stored In | 
| --- | --- | --- | 
| CREATING | The virtual tape is being created\. The tape can't be loaded into a tape drive, because the tape is being created\. | — | 
| AVAILABLE | The virtual tape is created and ready to be loaded into a tape drive\. | Amazon S3 | 
| IN TRANSIT TO VTS | The virtual tape has been ejected and is being uploaded for archive\. At this point, your Tape Gateway is uploading data to AWS\. If the amount of data being uploaded is small, this status might not appear\. When the upload is completed, the status changes to ARCHIVING\.  |  Amazon S3  | 
| ARCHIVING | The virtual tape is being moved by your Tape Gateway to the archive, which is backed by S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. This process happens after the data upload to AWS is completed\.  | Data is being moved from Amazon S3 to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. | 
| DELETING | The virtual tape is being deleted\. | Data is being deleted from Amazon S3 | 
| DELETED | The virtual tape has been successfully deleted\. | — | 
| RETRIEVING | The virtual tape is being retrieved from the archive to your Tape Gateway\.  The virtual tape can be retrieved only to a Tape Gateway\.   | Data is being moved from S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive to Amazon S3 | 
| RETRIEVED | The virtual tape is retrieved from the archive\. The retrieved tape is write\-protected\. | Amazon S3 | 
| RECOVERED |  The virtual tape is recovered and is read\-only\.  When your Tape Gateway is not accessible for any reason, you can recover virtual tapes associated with that Tape Gateway to another Tape Gateway\. To recover the virtual tapes, first disable the inaccessible Tape Gateway\.   | Amazon S3 | 
| IRRECOVERABLE | The virtual tape can't be read from or written to\. This status indicates an error in your Tape Gateway\. | Amazon S3 | 

### Determining Tape Status in an Archive<a name="determine-tape-status-vts"></a>

You can use the following procedure to determine the status of a virtual tape in an archive\.

**To determine the status of a virtual tape**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**\. 

1. In the **Status** column of the tape library grid, check the status of the tape\.

   The tape status also appears in the **Details** tab of each virtual tape\.

Following, you can find a description of the possible status values\.


| Status | Description | 
| --- | --- | 
| ARCHIVED | The virtual tape has been ejected and is uploaded to the archive\. | 
| RETRIEVING | The virtual tape is being retrieved from the archive\.  The virtual tape can be retrieved only to a Tape Gateway\.    | 
| RETRIEVED | The virtual tape has been retrieved from the archive\. The retrieved tape is read\-only\. | 

For additional information about how to work with tapes and VTL devices, see [Working With Tapes](managing-virtual-tapes-vtl.md)\.