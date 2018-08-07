# Creating an SMB File Share<a name="CreatingAnSMBFileShare"></a>

Creating an SMB accessible file share is a two\-step process\. Before you create an SMB file share, you configure the SMB settings for your file gateway for either Microsoft Active Directory \(AD\) or guest access\. A file share can provide one type of SMB access only\. After setting the authentication methods, you create your file share\.

**Note**  
An SMB file share doesn't operate correctly without the requisite ports open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

**To configure your SMB file share for Microsoft Active Directory access**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and on the **Gateway** page, select the box next to the file gateway that you want to join to a domain\.

1. For **Actions**, choose **Edit SMB Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SMBFileShareActionsMenu.png)

1. For Microsoft Active Directory authentication, choose **Join Domain**,

1. For **Domain name**, type your fully qualified domain name\.
**Note**  
You can use the [AWS Directory Service](https://aws.amazon.com/directoryservice/) to create a hosted Microsoft Active Directory domain service in the AWS Cloud\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Edit-SMB-Setttings-ad-db.png)

1. For **Domain user**, type your account name\. Your account must be able to join a server to a domain\.

1. Type your account password into the **Domain password** text box\.

1. Choose **Save** to complete the authentication\.

   A message at the top of the **Gateways** section of your console indicates that your gateway successfully joined your AD domain\.

**To configure your SMB file share for guest access**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and on the **Gateway** page, select the box next to the file gateway that you want to use for your guest file share\.

1. For **Actions**, choose **Edit SMB Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SMBFileShareActionsMenu.png)

   The **Edit SMB dialog box** appears as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Edit-SMB-Setttings-guest-db.png)

1. Select **Set guest password** to enable guest access for your SMB file share\. 
**Note**  
If you provide only guest access, your file gateway doesn't have to be part of an AD domain\. You can also use a file gateway that is a member of your Microsoft AD domain to create file shares with guest access\.

1. For Guest password, type a password that meets your organization's security requirements\. 

1. Choose **Save** to complete authentication\.

   A message at the top of the Gateways section of your console indicates that your gateway now allows guest access\.

In the next procedure, you create an SMB file share with either Microsoft Active Directory or guest access\. Make sure that you define the SMB file share settings for your file gateway before performing the following steps\.

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Shares**, select the file gateway that you want to use, and then choose **Create file share**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-step.png)

1. On the **Configure file share settings** page, for **Amazon S3 bucket name**, provide a name for an existing Amazon S3 bucket\. You use this bucket for your gateway to store files in and retrieve 

1. For **Access Objects using**, choose **Server Message Block \(SMB\)**\.

1. For **Gateway**, make sure that your gateway is chosen, and then choose **Next**\.

   The **Configure how files are stored** in Amazon S3 page appears, as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-storage-db.png)

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\.

   For more information, see [Storage Classes](http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file SMB file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your gateway to use to access your Amazon S3 bucket\. This role allows the gateway to access your S3 bucket\. A file gateway can create a new IAM role and access policy on your behalf\. Or, if you have an IAM role you want to use, you can specify it in the **IAM role** box and set up the access policy manually\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\. For information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

1. Choose **Next** to review configuration settings for your SMB file share, as shown in the figure following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-review-db.png)  
  


1. For Microsoft AD authentication, make sure that **Active Directory** appears for **Select authentication method**\. Microsoft AD access is the default authentication method\.
**Note**  
For Microsoft AD access, your file gateway must be joined to a domain\.  
For guest access, you must have set a guest access password\.   
Both access types are available at the same time\.

1. Choose **Read\-write** \(the default\) or **Read\-only**\. Choose **Close** to enforce your authentication settings\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your SMB file share is created, you can see your file share settings in its **Details** tab\.

The preceding procedure creates a Microsoft Active Directory file share\. Anyone with domain credentials can access this file share\. To limit access to certain users and groups, see [Editing Access Settings for Your SMB File Share](managing-gateway-file.md#enable-ad-settings)\.

**Next Step**

[Mounting Your SMB File Share on Your Client](using-smb-fileshare.md)