# Creating Tapes<a name="GettingStartedCreateTapes"></a>

In this section, you can find information about how to create new virtual backup tapes using AWS Storage Gateway\. You can create new virtual backup tapes manually with the AWS Storage Gateway Management Console, and you can also create them automatically\. 

AWS Storage Gateway automates creating new virtual tapes, helping eliminate the need for manual tape management, and making your large deployments simpler\. Automatic tape creation also helps scale on\-premises and archive storage needs\. 

**Note**  
You are charged only for the amount of data that you write to the tape, not the tape capacity\.  
You can use AWS Key Management Service \(AWS KMS\) to encrypt data written to a virtual tape that is stored in Amazon Simple Storage Service \(Amazon S3\)\. Currently, you can do this by using the AWS Storage Gateway API Reference\. For more information, see [CreateTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html) or [create\-tapes](https://docs.aws.amazon.com/cli/latest/reference/storagegateway/create-tapes.html)\.

**Topics**
+ [Creating Tapes Manually](#CreateTapesManually)
+ [Creating Tapes Automatically](#CreateTapesAutomatically)

## Creating Tapes Manually<a name="CreateTapesManually"></a>

Use the following steps to create virtual tapes manually using the console\.

**To create virtual tapes manually**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose **Create tapes** to open the **Create tapes** dialog box\.

1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

1. For **Number of tapes**, choose the number of tapes that you want to create\. For more information about tape quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

1. For **Capacity**, enter the size of the virtual tape that you want to create\. Tapes must be larger than 100 GiB\. For information about capacity quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tape will be stored when it is ejected by your backup software\. 

   Choose **Glacier Pool** if you want to archive the tape in GLACIER\. When your backup software ejects the tape, it is automatically archived in GLACIER\. You use S3 Glacier for more active archives where you can retrieve a tape typically within 3\-5 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon S3 Developer Guide*\.

   Choose **Deep Archive Pool** if you want to archive the tape in DEEP\_ARCHIVE\. When your backup software ejects the tape, the tape is automatically archived in DEEP\_ARCHIVE\. You use DEEP\_ARCHIVE for long\-term data retention and digital preservation where data is accessed once or twice a year\. You can retrieve a tape archived in DEEP\_ARCHIVE typically within 12 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon S3 Developer Guide*\.

   If you archive a tape in GLACIER, you can move it to DEEP\_ARCHIVE later\. For more information, see [Moving Your Tape from Glacier to Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in Amazon S3 Glacier when your backup software ejects it\.

1. \(Optional\) For **Tags**, enter a key and value to add tags to your tape\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your tapes\. 

1. Choose **Create tapes**\.

1. In the navigation pane, choose the **Tape Library** tab, and choose **Tapes** to see your tapes\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/created-tapes.png)

The status of the virtual tapes is initially set to **CREATING** when the virtual tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

## Creating Tapes Automatically<a name="CreateTapesAutomatically"></a>

 The tape gateway automatically creates new virtual tapes to maintain the minimum number of available tapes that you configure\. It then makes these new tapes available for import by the backup application so that your backup jobs can run without interruption\. Automatic tape creation removes the need for custom scripting in addition to the manual process of creating new virtual tapes\. 

**To enable automatic tape creation**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose the gateway that you want to automatically create tapes for\.

1. In the **Actions** menu, choose **Configure tape auto\-create**\. 

   A new tab, **Tape auto\-create**, appears in the **Details** pane\. You can configure, change, or delete tape auto\-create options here\.

1. To enable automatic tape creation, choose the **Tape auto\-create** tab, and then choose **Configure tape auto\-create**\. You can change the settings for automatic tape creation here\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-automatic-tape-creation.png)

1. For **Minimum number of tapes**, enter the minimum number of virtual tapes that should be available on the tape gateway at all times\. The valid range for this value is a minimum of 1 and a maximum of 10\. 

1. For **Capacity**, enter the size, in bytes, of the virtual tape capacity\. The valid range is a minimum of 100 Gib and a maximum of 5 TiB\.

1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode, and you can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tapes will be stored when they are ejected by your backup software\. 

   Choose **Glacier Pool** if you want to archive the tapes in GLACIER\. When your backup software ejects the tapes, they are automatically archived in GLACIER\. You use S3 Glacier for more active archives where you can retrieve a tape typically within 3\-5 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon S3 Developer Guide*\.

   Choose **Deep Archive Pool** if you want to archive the tapes in DEEP\_ARCHIVE\. When your backup software ejects the tape, the tape is automatically archived in DEEP\_ARCHIVE\. You use DEEP\_ARCHIVE for long\-term data retention and digital preservation where data is accessed once or twice a year\. You can retrieve a tape archived in DEEP\_ARCHIVE typically within 12 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon S3 Developer Guide*\.

   If you archive tapes in GLACIER, you can move them to DEEP\_ARCHIVE later\. For more information, see [Moving Your Tape from Glacier to Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.

1. In the navigation pane, choose the **Tape Library** tab, and then choose **Tapes** to see your tapes\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/created-tapes.png)

   The status of available virtual tapes is initially set to **CREATING** when the tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

   For more information about changing automatic tape creation policies, or deleting automatic tape creation from a tape gateway, see [Managing Automatic Tape Creation](managing-gateway-vtl.md#managing-automatic-tape-creation)\.

**Next Step**

[Using Your Tape Gateway](GettingStarted-create-tape-gateway.md)