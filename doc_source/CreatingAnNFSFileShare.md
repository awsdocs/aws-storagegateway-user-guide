# Creating an NFS File Share<a name="CreatingAnNFSFileShare"></a>

Use the following procedure to create an NFS file share\.

**To create an NFS file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Create file share**\.

1. For **Amazon S3 bucket name**, provide the name for the Amazon S3 bucket for your gateway to store your files in and retrieve your files to\. This name must be compliant with Domain Name Service \(DNS\)\. This bucket must also exist already in S3; it isn't created for you by your file gateway\. For information on DNS\-compliant names for buckets, see [Rules for Bucket Naming](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/create-file-share-3.png)

1. For **Access objects using**, choose **Network File System\(NFS\)**\.

1. For **Gateway**, choose your file gateway from the list and choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/create-file-share-2.png)

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\.

   For more information, see [Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file NFS file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your gateway to use to access your Amazon S3 bucket\. This role allows the gateway to access your S3 bucket\. A file gateway can create a new IAM role and access policy on your behalf\. Or, if you have an IAM role that you want to use, you can specify it in the **IAM role** box and set up the access policy manually\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\. For information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\. 

1. Choose **Next** to review configuration settings for your file share\. You can change the allowed NFS clients for **Allowed clients** as needed\. 

   To change **Squash level** and **Export as** under **Mount options** and to change **File metadata defaults** options, choose **Edit** by the option to change\.
**Note**  
For file shares mounted on a Microsoft Windows client, if you choose **Read\-only** for **Export as**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/review-file-share.png)  
  


The next step is to review configuration settings for your file share\. Your file gateway applies default settings to your file share\.

**To change the configuration settings for your NFS file share:**

1. Choose **Edit** for the settings that you want to change\.

1. Configure **Allowed clients** to allow or restrict each client's access to your file share\. For more information, see [Editing Access Settings for Your NFS File Share](managing-gateway-file.md#edit-nfs-client)\.

1. \(Optional\) Modify the mount options for your file share as needed\. 

1. \(Optional\) Modify the file metadata defaults as needed\. For more information, see [Editing Metadata Defaults for Your NFS File Share](managing-gateway-file.md#edit-metadata-defaults)\.

1. Review your file share configuration settings, and then choose **Create file share**\. 

   After your NFS file share is created, you can see your file share settings in the file share's **Details** tab\.

**Next Step**

[Mounting Your NFS File Share on Your Client](GettingStartedAccessFileShare.md)