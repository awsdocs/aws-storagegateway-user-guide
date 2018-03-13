# Creating a File Share<a name="GettingStartedCreateFileShare"></a>

In the following section, you can find instructions about how to create a file share\.

**Important**  
To create a file share, a file gateway requires you to activate AWS Security Token Service \(AWS STS\)\. Make sure that AWS STS is activated in the AWS Region that you are creating your file gateway in\. If AWS STS is not activated in that AWS Region, activate it\. For information about how to activate AWS STS, see [Activating and Deactivating AWS STS in an AWS Region](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

**To create a file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **File shares**, and then choose **Create file share**\. You can also choose **Create file share** from the **Gateway** tab\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/create-file-share.png)

1. In the **Create file share** wizard, choose your gateway from the **Gateway** list\.

1. For **Amazon S3 bucket name**, provide a name for the Amazon S3 bucket for your gateway to store your files in and retrieve your files to\. This name must be compliant with Domain Name Service \(DNS\)\. This bucket must also exist already; it isn't created for you\. For information on DNS\-compliant names for buckets, see [Rules for Bucket Naming](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket\.

1. For **Object metadata**, choose the metadata you want to use\.

   Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.

   Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file NFS file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](managing-gateway-file.md#cross-account-access)\.

   Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your gateway to use to access your Amazon S3 bucket\. This role allows the gateway to access your S3 bucket\. A file gateway can create a new IAM role and access policy on your behalf\. Or, if you have an IAM role you want to use, you can specify it in the **IAM role** box and set up the access policy manually\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\. For information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\. 

1. Choose **Next** to review configuration settings for your file share\. You can change the allowed NFS clients for **Allowed clients** as needed\. 

   To change **Squash level** and **Export as** under **Mount options** and to change **File metadata defaults** options, choose **Edit** by the option to change\.
**Note**  
For file shares mounted on a Microsoft Windows client, if you choose **Read\-only** for **Export as**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/review-file-share.png)  
  


1. Configure **Allowed clients** to allow or restrict each client's access to your file share\. For more information, see [Editing Allowed NFS Clients](managing-gateway-file.md#edit-nfs-client)\.

1. \(Optional\) Modify the mount options for your file share as needed\. For more information, see [Updating a File Share](managing-gateway-file.md#update-file-share)\.

1. \(Optional\) Modify the file metadata defaults as needed\. For more information, see [Editing Metadata Defaults](managing-gateway-file.md#edit-metadata-defaults)\.

1. Review your file share configuration settings, and then choose **Create file share**\. 

   After your file share is created, you can see your file share settings in the file share's **Details** tab\.

**Next Step**

[Using Your File Share](getting-started-use-fileshare.md)