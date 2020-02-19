# Creating Tapes<a name="GettingStartedCreateTapes"></a>

**Note**  
You are charged only for the amount of data you write to the tape, not the tape capacity\.  
You can use AWS Key Management Service \(AWS KMS\) to encrypt data written to a virtual tape that is stored in Amazon S3\. Currently, you can do this by using the AWS Storage Gateway API Reference\. For more information, see [CreateTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html) or [create\-tapes](https://docs.aws.amazon.com/cli/latest/reference/storagegateway/create-tapes.html)\.

**To create virtual tapes**

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose **Create tapes** to open the **Create tapes** dialog box\.

1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

1. For **Number of tapes**, choose the number of tapes you want to create\. For more information about tape quotas, see [AWS Storage Gateway Quotas](resource-gateway-limits.md)\.

1. For **Capacity**, type the size of the virtual tape you want to create\. Tapes must be larger than 100GiB\. For information about capacity quotas, see [AWS Storage Gateway Quotas](resource-gateway-limits.md)\.

1. For **Barcode prefix**, type the prefix you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode\. You can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tape will be stored when it is ejected by your backup software\. 

   Choose **S3 Glacier Pool** if you want to archive the tape in GLACIER\. When your backup software ejects the tape, it is automatically archived in GLACIER\. You use S3 Glacier for more active archives where you can retrieve a tape typically within 3\-5 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)   

   Choose **Deep Archive Pool** if you want to archive the tape in DEEP\_ARCHIVE\. When your backup software ejects the tape, the tape is automatically archived in DEEP\_ARCHIVE\. You use DEEP\_ARCHIVE for long\-term data retention and digital preservation where data is accessed once or twice a year\. You can retrieve a tape archived in DEEP\_ARCHIVE typically within 12 hours\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)\.

   If you archive a tape in GLACIER, you can move it to DEEP\_ARCHIVE later\. For more information, see [Moving Your Tape from Glacier to Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in Amazon S3 Glacier when your backup software ejects it\.

1. \(Optional\) For **Tags**, enter a key and value to add tags to your tape\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your tapes\. 

1. Choose **Create tapes**\.

1. In the navigation pane, choose the **Tape Library** tab and choose **Tapes** to see your tapes\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/created-tapes.png)

The status of the virtual tapes is initially set to **CREATING** when the virtual tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

**Next Step**

[Using Your Tape Gateway](GettingStarted-create-tape-gateway.md)