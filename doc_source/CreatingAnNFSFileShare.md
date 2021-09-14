# Creating an NFS file share<a name="CreatingAnNFSFileShare"></a>

Use the following procedure to create an NFS file share\.

**To create an NFS file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share**\.

1. On the **Configure file share settings** page, for **Amazon S3 bucket name / Prefix name**, do one or more of the following:
   + In *Existing S3 bucket name*, enter the name for an existing Amazon S3 bucket\. You use this bucket for your gateway to store files in and retrieve\. For information on creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.
   + \(Optional\) In *S3 prefix name*, enter a prefix name for the S3 bucket\.
**Note**  
The prefix name must end with a "/"\.
If you enter a prefix name, you must enter a file share name\.
Once the file share is created, the prefix name can't be modified or deleted\.

1. \(Optional\) For **File share name**, enter a name for the file share\. The default name is the S3 bucket name\.
**Note**  
If you enter a prefix name, you must enter a file share name\.
Once the file share is created, the file share name can't be deleted\.

1. For **Access objects using**, choose **Network File System \(NFS\)**\.

1. For **Gateway**, choose your file gateway from the list\.

1. \(Optional\) For **Automated cache refresh from S3 after**, choose the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh after which access to the directory would cause the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file NFS file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your file gateway to use to access your Amazon S3 bucket:
   + **Create a new IAM role** to enable the file gateway to create a new IAM role and access the policy on your behalf\.
   + **Use an existing IAM role** to choose an existing IAM role and to set up the access policy manually\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your file gateway stores in Amazon S3:
   + Choose **S3\-Managed Keys \(SSE\-S3\)** to use server\-side encryption managed with Amazon S3 \(SSE\-S3\)\.
   + Choose **KMS\-Managed Keys \(SSE\-KMS\)** to use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\)\. In the **Master key** box, choose an existing master KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric customer master keys \(CMKs\) are not supported\.

1. Choose **Next** to review configuration settings for your file share\. Your file gateway applies default settings to your file share\.

**To change the configuration settings for your NFS file share**

1. Choose **Edit** for the settings that you want to change\. Choose **Close** to enforce your settings\.

1. For **Allowed clients**, configure to allow or restrict each client's access to your file share\. Provide the IP address or CIDR notation for the client that you want to allow\. For information about supported NFS clients, see [Supported NFS clients for a file gateway](Requirements.md#requirements-nfs-clients)\.

1. For **Mount options**, you can edit **Squash level** and **Export as**\.

   For **Squash level**, choose one of the following:
   + **All squash**: All user access is mapped to User ID \(UID\) \(65534\) and Group ID \(GID\) \(65534\)\.
   + **No root squash**: The remote superuser \(root\) receives access as root\.
   + **Root squash \(default\)**: Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.

   For **Export as**, choose one of the following:
   + **Read\-write**
   + **Read\-only**
**Note**  
For file shares mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

1. For **File metadata defaults**, you can edit the **Directory permissions**, **File permissions**, **User ID**, and **Group ID**\. For more information, see [Editing metadata defaults for your NFS file share](managing-gateway-file.md#edit-metadata-defaults)\.

1. For **Tags**, you can add new tags or remove existing tags\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your NFS file share is created, you can see your file share settings in the file share's **Details** tab\.

**Next Step**

[Mounting your NFS file share on your client](GettingStartedAccessFileShare.md)