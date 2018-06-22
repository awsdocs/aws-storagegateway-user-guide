# Working With Tapes<a name="managing-virtual-tapes-vtl"></a>

AWS Storage Gateway provides one virtual tape library \(VTL\) for each tape gateway you activate\. Initially, the library contains no tapes, but you can create tapes whenever you need to\. Your application can read and write to any tapes available on your tape gateway\. A tape's status must be AVAILABLE for you to write to the tape\. These tapes are backed by Amazon Simple Storage Service \(Amazon S3\)—that is, when you write to these tapes, the tape gateway stores data in Amazon S3\. For more information, see [Understanding Tape Status Information in a VTL](managing-gateway-vtl.md#tape-status)\.

**Topics**
+ [Archiving Tapes](#main-archiving-tapes-managing-vtl)
+ [Canceling Tape Archival](#main-canceling-archival-vtl)

The tape library shows tapes in your tape gateway\. The library shows the tape barcode, status, and size, amount of the tape used, and the gateway the tape is associated with\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/created-tapes.png)

When you have a large number of tapes in the library, the console supports searching for tapes by barcode, by status, or by both\. When you search by barcode, you can filter by status and gateway\.

**To search by barcode, status, and gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tapes**, and then type a value in the search box\. The value can be the barcode, status, or gateway\. By default, AWS Storage Gateway searches for all virtual tapes\. However, you can also filter your search by status\.

   If you filter for status, tapes that match your criteria appear in the library in the AWS Storage Gateway console\.

   If you filter for gateway, tapes that are associated with that gateway appear in the library in the AWS Storage Gateway console\.
**Note**  
By default, AWS Storage Gateway displays all tapes regardless of status\.

## Archiving Tapes<a name="main-archiving-tapes-managing-vtl"></a>

You can archive the virtual tapes that are in your tape gateway\. When you archive a tape, AWS Storage Gateway moves the tape to the archive\.

To archive a tape, you use your backup software\. Tape archival process consists of three stages, seen as the tape statuses **IN TRANSIT TO VTS**, **ARCHIVING**, and **ARCHIVED**: 
+ To archive a tape, use the command provided by your backup application\. When the archival process begins the tape status changes to **IN TRANSIT TO VTS** and the tape is no longer accessible to your backup application\. In this stage, your tape gateway is uploading data to AWS\. If needed, you can cancel the archival in progress\. For more information about canceling archival, see [Canceling Tape Archival](#main-canceling-archival-vtl)\. 
**Note**  
The steps for archiving a tape depend on your backup application\. For detailed instructions, see the documentation for your backup application\.
+  After the data upload to AWS completes, the tape status changes to **ARCHIVING** and AWS Storage Gateway begins moving the tape to the archive\. You cannot cancel the archival process at this point\. 
+ After the tape is moved to the archive, its status changes to **ARCHIVED** and you can retrieve the tape to any of your gateways\. For more information about tape retrieval, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\. 

The steps involved in archiving a tape depend on your backup software\. For instructions on how to archive a tape by using Symantec NetBackup software, see [Archiving the Tape](backup_netbackup-vtl.md#GettingStarted-archiving-tapes-vtl)\.

## Canceling Tape Archival<a name="main-canceling-archival-vtl"></a>

 After you start archiving a tape, you might decide you need your tape back\. For example, you might want to cancel the archival process, get the tape back because the archival process is taking too long, or read data from the tape\. A tape that is being archived goes through three statuses, as shown following:
+ IN TRANSIT TO VTS: Your tape gateway is uploading data to AWS\. 
+ ARCHIVING: Data upload is complete and the tape gateway is moving the tape to the archive\.
+ ARCHIVED: The tape is moved and the archive and is available for retrieval\.

You can cancel archival only when the tape's status is IN TRANSIT TO VTS\. Depending on factors such as upload bandwidth and the amount of data being uploaded, this status might or might not be visible in the AWS Storage Gateway console\. To cancel a tape archival, use the [CancelRetrieval](http://docs.aws.amazon.com/storagegateway/latest/APIReference//API_CancelRetrieval.html) action in the API reference\. 