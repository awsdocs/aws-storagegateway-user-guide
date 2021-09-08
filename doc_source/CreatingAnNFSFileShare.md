# Create an NFS file share<a name="CreatingAnNFSFileShare"></a>

Use the following procedure to create an NFS file share\.

**Note**  
When a file is written to the file gateway by an NFS client, the file gateway uploads the file's data to Amazon S3 followed by its metadata \(ownerships, timestamps, etc\.\)\. Uploading the file data creates an S3 object, and uploading the metadata for the file updates the metadata for the S3 object\. This process creates another version of the object, resulting in two versions of an object\. If S3 Versioning is enabled, both versions will be stored\.  
If you change the metadata of a file stored in your file gateway, a new S3 object is created and replaces the existing S3 object\. This behavior is different from editing a file in a file system, where editing a file does not result in a new file being created\. You should test all file operations that you plan to use with Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
The use of S3 Versioning and Cross\-Region replication \(CRR\) in Amazon S3 should be carefully considered when data is being uploaded from your file gateway\. Uploading files from your file gateway to Amazon S3 when S3 Versioning is enabled results in at least two versions of an S3 object\.  
Certain workflows involving large files and file\-writing patterns such as file uploads that are performed in several steps can increase the number of stored S3 object versions\. If the file gateway cache needs to free up space due to high file\-write rates, multiple S3 object versions might be created\. These scenarios increase S3 storage if S3 Versioning is enabled and increase transfer costs associated with CRR\. You should test all file operations that you plan to use with Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
Using the Rsync utility with your file gateway results in the creation of temporary files in the cache and the creation of temporary S3 objects in Amazon S3\. This situation results in early deletion charges in the S3 Standard\-Infrequent Access \(S3 Standard\-IA\) and S3 Intelligent\-Tiering storage classes\.

------
#### [ New console ]

**To create an NFS file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share** to open the **File share settings** page\.

1. For **Gateway**, choose your Amazon S3 File Gateway from the list\.

1. For **Amazon S3 location**, do one of the following:
   + To connect the file share directly to an S3 bucket, choose **S3 bucket name**, then enter the S3 bucket name and, optionally, a prefix name for objects created by the file share\. Your gateway uses this bucket to store and retrieve files\. For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon Simple Storage Service Console User Guide*\.
   + To connect the file share to an S3 bucket through an access point, choose **S3 access point**, then enter the S3 access point name and, optionally, a prefix name for objects created by the file share\. Your bucket policy must be configured to delegate access control to the access point\. For information about access points, see [Managing data access with Amazon S3 access points](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points.html) and [Delegating access control to access points](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points-policies.html#access-points-delegating-control) in the *Amazon Simple Storage Service User Guide*\. For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon Simple Storage Service Console User Guide*\.
**Note**  
If you enter a prefix name or choose to connect through an access point, you must enter a file share name\.
The prefix name must end with a forward slash \(`/`\)\.
After the file share is created, the prefix name can't be modified or deleted\.

1. For **AWS Region**, choose the AWS Region of the S3 bucket\.

1. For **File share name**, enter a name for the file share\. The default name is the S3 bucket name or access point name\.
**Note**  
If you entered a prefix name, you must enter a file share name\.
After the file share is created, the file share name can't be deleted\.

1. \(Optional\) For **AWS PrivateLink for S3**, do the following:

   1. To configure the file share to connect to S3 through an interface endpoint in your VPC powered by AWS PrivateLink, select **Use VPC endpoint**\.

       

   1. Choose either **VPC endpoint ID** or **VPC endpoint DNS name** to identify the VPC interface endpoint that you want the file share to connect through, and then provide the required information in the corresponding field\.
**Note**  
This step is required if the file share connects to S3 through a VPC access point\.
File share connections using AWS PrivateLink are not supported on FIPS gateways\.
For information about AWS PrivateLink, see [AWS PrivateLink for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) in the *Amazon Simple Storage Service User Guide*\.

1. For **Access objects using**, choose **Network File System \(NFS\)**\.

1. For **Automated cache refresh from S3 after**, choose **Set refresh interval**, and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. For **File upload notification**, choose **Settling time \(seconds\)** to be notified when a file has been fully uploaded to S3 by the file gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.

   To help monitor your S3 billing, use AWS Trusted Advisor\. For more information, see [Monitoring tools](https://docs.aws.amazon.com/AmazonS3/latest/userguide/monitoring-automated-manual.html) in the *Amazon Simple Storage Service User Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the NFS file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of the bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your file gateway to use to access your Amazon S3 bucket:
   + Choose **Create a new IAM role** to enable the file gateway to create a new IAM role and access the policy on your behalf\.
   + Choose **Use an existing IAM role** to choose an existing IAM role and to set up the access policy manually\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your file gateway stores in Amazon S3:
   + Choose **S3\-Managed Keys \(SSE\-S3\)** to use server\-side encryption managed with Amazon S3 \(SSE\-S3\)\.
   + Choose **KMS\-Managed Keys \(SSE\-KMS\)** to use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\)\. In the **Master key** box, choose an existing master KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, see [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias that is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric customer master keys \(CMKs\) are not supported\.

1. Choose **Next** to configure file access settings\.

**To configure file access settings**

1. For **Allowed clients**, specify whether to allow or restrict each client's access to your file share\. Provide the IP address or CIDR notation for the clients that you want to allow\. For information about supported NFS clients, see [Supported NFS clients for a file gateway](Requirements.md#requirements-nfs-clients)\.

1. For **Mount options**, specify the options that you want for **Squash level** and **Export as**\.

   For **Squash level**, choose one of the following:
   + **All squash**: All user access is mapped to User ID \(UID\) \(65534\) and Group ID \(GID\) \(65534\)\.
   + **No root squash**: The remote superuser \(root\) receives access as root\.
   + **Root squash \(default\)**: Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.

   For **Export as**, choose one of the following:
   + **Read\-write**
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

1. For **File metadata defaults**, you can edit the **Directory permissions**, **File permissions**, **User ID**, and **Group ID**\. For more information, see [Editing metadata defaults for your NFS file share](managing-gateway-file.md#edit-metadata-defaults)\.

1. Choose **Next**\.

1. Review your file share configuration settings, and then choose **Finish**\.

   After your NFS file share is created, you can see your file share settings in the file share's **Details** tab\.

------
#### [ Original console ]

**To create an NFS file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share**\.

1. On the **Configure file share settings** page, for **Amazon S3 bucket name / Prefix name**, do one or more of the following:
   + In *Existing S3 bucket name*, enter the name for an existing Amazon S3 bucket\. You use this bucket for your gateway to store files in and retrieve\. For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.
   + \(Optional\) In *S3 prefix name*, enter a prefix name for the S3 bucket\. For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon Simple Storage Service Console User Guide*\.
**Note**  
The prefix name must end with a forward slash \(`/`\)\.
If you enter a prefix name, you must enter a file share name\.
After the file share is created, the prefix name can't be modified or deleted\.

1. \(Optional\) For **File share name**, enter a name for the file share\. The default name is the S3 bucket name\.
**Note**  
If you enter a prefix name, you must enter a file share name\.
After the file share is created, the file share name can't be deleted\.

1. For **Access objects using**, choose **Network File System \(NFS\)**\.

1. For **Gateway**, choose your file gateway from the list\.

1. \(Optional\) For **Automated cache refresh from S3 after**, choose the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. \(Optional\) For **File upload notification**, choose the check box to be notified when a file has been fully uploaded to S3 by the file gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.

   To help monitor your S3 billing, use AWS Trusted Advisor\. For more information, see [Monitoring tools](https://docs.aws.amazon.com/AmazonS3/latest/userguide/monitoring-automated-manual.html) in the *Amazon Simple Storage Service User Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the NFS file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of the bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your file gateway to use to access your Amazon S3 bucket:
   + Choose **Create a new IAM role** to enable the file gateway to create a new IAM role and access the policy on your behalf\.
   + Choose **Use an existing IAM role** to choose an existing IAM role and to set up the access policy manually\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your file gateway stores in Amazon S3:
   + Choose **S3\-Managed Keys \(SSE\-S3\)** to use server\-side encryption managed with Amazon S3 \(SSE\-S3\)\.
   + Choose **KMS\-Managed Keys \(SSE\-KMS\)** to use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\)\. In the **Master key** box, choose an existing master KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, see [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias that is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric customer master keys \(CMKs\) are not supported\.

1. Choose **Next** to review the configuration settings for your file share\. Your file gateway applies default settings to your file share\.

------

------
#### [ New Console ]

**To edit the configuration settings for your NFS file share**

1. Choose **Edit** for the settings that you want to change\.

1. For **File share details**, choose **Edit**, make the changes that you want, and then choose **Next**\.

1. For **Amazon S3 storage settings**, choose **Edit**, make the changes that you want, and then choose **Next**\.

1. For **File access settings**,choose **Edit**, make the changes that you want, and then choose **Next**\.

1. For **Access object**, configure whether to allow or restrict each client's access to your file share\. Provide the IP address or CIDR notation for the clients that you want to allow\. For information about supported NFS clients, see [Supported NFS clients for a file gateway](Requirements.md#requirements-nfs-clients)\.

1. For **Mount options**, specify the options that you want for **Squash level** and **Export as**\.

   For **Squash level**, choose one of the following:
   + **All squash**: All user access is mapped to User ID \(UID\) \(65534\) and Group ID \(GID\) \(65534\)\.
   + **No root squash**: The remote superuser \(root\) receives access as root\.
   + **Root squash \(default\)**: Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.

   For **Export as**, choose one of the following:
   + **Read\-write**
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

1. For **File metadata defaults**, you can edit the **Directory permissions**, **File permissions**, **User ID**, and **Group ID**\. For more information, see [Editing metadata defaults for your NFS file share](managing-gateway-file.md#edit-metadata-defaults)\.

1. When you are done, choose **Create** to create your file share\.

------
#### [ Original Console ]

**To change the configuration settings for your NFS file share**

1. Choose **Edit** for the settings that you want to change\. Choose **Close** to enforce your settings\.

1. For **Allowed clients**, configure whether to allow or restrict each client's access to your file share\. Provide the IP address or CIDR notation for the clients that you want to allow\. For information about supported NFS clients, see [Supported NFS clients for a file gateway](Requirements.md#requirements-nfs-clients)\.

1. For **Mount options**, specify the options that you want for **Squash level** and **Export as**\.

   For **Squash level**, choose one of the following:
   + **All squash**: All user access is mapped to User ID \(UID\) \(65534\) and Group ID \(GID\) \(65534\)\.
   + **No root squash**: The remote superuser \(root\) receives access as root\.
   + **Root squash \(default\)**: Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.

   For **Export as**, choose one of the following:
   + **Read\-write**
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

1. For **File metadata defaults**, you can edit the **Directory permissions**, **File permissions**, **User ID**, and **Group ID**\. For more information, see [Editing metadata defaults for your NFS file share](managing-gateway-file.md#edit-metadata-defaults)\.

1. \(Optional\) For **Tags**, you can add new tags or remove existing tags\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your NFS file share is created, you can see your file share settings in the file share's **Details** tab\.

------

**Next Step**

[Mount your NFS file share on your client](GettingStartedAccessFileShare.md)