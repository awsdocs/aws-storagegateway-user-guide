--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Creating Tapes<a name="GettingStartedCreateTapes"></a>

In this section, you can find information about how to create new virtual tapes using AWS Storage Gateway\. You can create new virtual tapes manually with the AWS Storage Gateway console, and you can also create them automatically\. 

Storage Gateway automates creating new virtual tapes, helping decrease the need for manual tape management, and making your large deployments simpler\. Automatic tape creation also helps scale on\-premises and archive storage needs\. 

Storage Gateway supports *write once, read many* \(WORM\) and *tape retention lock* on virtual tapes\. WORM\-enabled virtual tapes help ensure that the data on active tapes in your virtual tape library cannot be overwritten or erased\. For more information about WORM protection for virtual tapes, see the section following, [Write Once, Read Many \(WORM\) Tape Protection](#WORM)\. 

With tape retention lock, you can specify the retention mode and period on archived virtual tapes, preventing them from being deleted for a fixed amount of time up to 100 years\. It includes permission controls on who can delete tapes or modify the retention settings\. For more information about tape retention lock, see [Using Tape Retention Lock](CreatingCustomTapePool.md#TapeRetentionLock)\. 

**Note**  
You are charged only for the amount of data that you write to the tape, not the tape capacity\.  
You can use AWS Key Management Service \(AWS KMS\) to encrypt data written to a virtual tape that is stored in Amazon Simple Storage Service \(Amazon S3\)\. Currently, you can do this by using the AWS Storage Gateway API or AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html) or [create\-tapes](https://docs.aws.amazon.com/cli/latest/reference/storagegateway/create-tapes.html)\.

**Topics**
+ [Write Once, Read Many \(WORM\) Tape Protection](#WORM)
+ [Creating Tapes Manually](#CreateTapesManually)
+ [Creating Tapes Automatically](#CreateTapesAutomatically)

## Write Once, Read Many \(WORM\) Tape Protection<a name="WORM"></a>

You can prevent virtual tapes from being overwritten or erased by enabling WORM protection for virtual tapes in AWS Storage Gateway\. WORM protection for virtual tapes is enabled when creating tapes\.

Data that is written to WORM virtual tapes can't be overwritten\. Only new data can be appended to WORM virtual tapes, and existing data can't be erased\. Enabling WORM protection for virtual tapes helps protect those tapes while they are in active use, before they are ejected and archived\. 

WORM configuration can only be set when tapes are created, and that configuration cannot be changed after the tapes are created\.

## Creating Tapes Manually<a name="CreateTapesManually"></a>

Use the following steps to create virtual tapes manually using the console\.

**To create virtual tapes manually**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose **Create tapes** to open the **Create tapes** pane\.

1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

1. For **Tape type**, choose **Standard** to create standard virtual tapes\. Choose **WORM** to create *write once read many* \(WORM\) virtual tapes\. 

1. For **Number of tapes**, choose the number of tapes that you want to create\. For more information about tape quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

1. For **Capacity**, enter the size of the virtual tape that you want to create\. Tapes must be larger than 100 GiB\. For information about capacity quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. You can use a prefix to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool**, **Deep Archive Pool**, or a custom pool that you have created\. The pool determines the storage class in which your tape is stored when it is ejected by your backup software\. 
   + Choose **Glacier Pool** if you want to archive the tape in the S3 Glacier Flexible Retrieval storage class\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives, where you can retrieve a tape typically within 3\-5 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose **Deep Archive Pool** if you want to archive the tape in the S3 Glacier Deep Archive storage class\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation, where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose a custom pool, if any are available\. You configure custom tape pools to use either **Deep Archive Pool** or **Glacier Pool**\. Tapes are archived to the configured storage class when they are ejected by your backup software\. 

   If you archive a tape in S3 Glacier Flexible Retrieval, you can move it to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects them\.

1. \(Optional\) For **Tags**, choose **Add new tag** and enter a key and value to add tags to your tape\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your tapes\. 

1. Choose **Create tapes**\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

The status of the virtual tapes is initially set to **CREATING** when the virtual tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

## Creating Tapes Automatically<a name="CreateTapesAutomatically"></a>

 The tape gateway automatically creates new virtual tapes to maintain the minimum number of available tapes that you configure\. It then makes these new tapes available for import by the backup application so that your backup jobs can run without interruption\. Automatic tape creation removes the need for custom scripting in addition to the manual process of creating new virtual tapes\. 

The tape gateway spawns a new tape automatically when it has fewer tapes than the minimum number of available tapes specified for automatic tape creation\. A new tape is spawned when:
+ A tape is imported from an import/export slot\.
+ A tape is imported to the tape drive\.

The gateway maintains a minimum number of tapes with the barcode prefix specified in the automatic tape creation policy\. If there are fewer tapes than the minimum number of tapes with the barcode prefix, the gateway automatically creates enough new tapes to equal the minimum number of tapes specified in the automatic tape creation policy\.

When you eject a tape and it goes into the import/export slot, that tape does not count toward the minimum number of tapes specified in your automatic tape creation policy\. Only tapes in the import/export slot are counted as being "available\." Exporting a tape does not trigger automatic tape creation\. Only imports affect the number of available tapes\.

Moving a tape from the import/export slot to a tape drive or storage slot reduces the number of tapes in the import/export slot with the same barcode prefix\. The gateway creates new tapes to maintain the minimum number of available tapes for that barcode prefix\.

**To enable automatic tape creation**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway that you want to automatically create tapes for\.

1. In the **Actions** menu, choose **Configure tape auto\-create**\. 

   The **Tape auto\-create** page appears\. You can add, change, or remove tape auto\-create options here\.

1. To enable automatic tape creation, choose **Add new item** then configure the settings for automatic tape creation\. 

1. For **Tape type**, choose **Standard** to create standard virtual tapes\. Choose **WORM** to create *write\-once\-read\-many* \(WORM\) virtual tapes\. 

1. For **Minimum number of tapes**, enter the minimum number of virtual tapes that should be available on the tape gateway at all times\. The valid range for this value is a minimum of 1 and a maximum of 10\. 

1. For **Capacity**, enter the size, in bytes, of the virtual tape capacity\. The valid range is a minimum of 100 GiB and a maximum of 5 TiB\.

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool**, **Deep Archive Pool**, or a custom pool that you have created\. The pool determines the storage class in which your tape is stored when it is ejected by your backup software\. 
   + Choose **Glacier Pool** if you want to archive the tape in the S3 Glacier Flexible Retrieval storage class\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives, where you can retrieve a tape typically within 3\-5 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose **Deep Archive Pool** if you want to archive the tape in the S3 Glacier Deep Archive storage class\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation, where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.
   + Choose a custom pool, if any are available\. You configure custom tape pools to use either **Deep Archive Pool** or **Glacier Pool**\. Tapes are archived to the configured storage class when they are ejected by your backup software\. 

   If you archive a tape in S3 Glacier Flexible Retrieval, you can move it to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects them\.

1. When finished configuring settings, choose **Save changes**\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   The status of available virtual tapes is initially set to **CREATING** when the tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

   For more information about changing automatic tape creation policies, or deleting automatic tape creation from a tape gateway, see [Managing Automatic Tape Creation](managing-gateway-vtl.md#managing-automatic-tape-creation)\.

**Next Step**

[Using Your Tape Gateway](GettingStarted-create-tape-gateway.md)