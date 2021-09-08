# Create a file share<a name="GettingStartedCreateFileShare"></a>

In this section, you can find instructions on how to create a file share\. You can create a file share that can be accessed using either the Network File System \(NFS\) or Server Message Block \(SMB\) protocol\.

**Note**  
When a file is written to the file gateway by an NFS or SMB client, the file gateway uploads the file's data to Amazon S3 followed by its metadata \(ownerships, timestamps, etc\.\)\. Uploading the file data creates an S3 object, and uploading the metadata for the file updates the metadata for the S3 object\. This process creates another version of the object, resulting in two versions of an object\. If S3 Versioning is enabled, both versions will be stored\.  
If you change the metadata of a file stored in your file gateway, a new S3 object is created and replaces the existing S3 object\. This behavior is different from editing a file in a file system, where editing a file does not result in a new file being created\. You should test all file operations that you plan to use with AWS Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
The use of S3 Versioning and Cross\-Region replication \(CRR\) in Amazon S3 should be carefully considered when data is being uploaded from your file gateway\. Uploading files from your file gateway to Amazon S3 when S3 Versioning is enabled results in at least two versions of an S3 object\.  
Certain workflows involving large files and file\-writing patterns such as file uploads that are performed in several steps can increase the number of stored S3 object versions\. If the file gateway cache needs to free up space due to high file\-write rates, multiple S3 object versions might be created\. These scenarios increase S3 storage if S3 Versioning is enabled and increase transfer costs associated with CRR\. You should test all file operations you plan to use with Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
Using the Rsync utility with your file gateway results in the creation of temporary files in the cache and the creation of temporary S3 objects in Amazon S3\. This situation results in early deletion charges in the S3 Standard\-Infrequent Access \(S3 Standard\-IA\) and S3 Intelligent\-Tiering storage classes\.

When you create an NFS share, by default anyone who has access to the NFS server can access the NFS file share\. You can limit access to clients by IP address\.

For SMB, you can have one of three different modes of authentication:
+ A file share with Microsoft Active Directory \(AD\) access\. Any authenticated Microsoft AD user gets access to this file share type\.
+ An SMB file share with limited access\. Only certain domain users and groups that you specify are allowed access \(through an allow list\)\. Users and groups can also be denied access \(through a deny list\)\.
+ An SMB file share with guest access\. Any users who can provide the guest password get access to this file share\.
**Note**  
File shares exported through the gateway for NFS file shares support POSIX permissions\. For SMB file shares, you can use access control lists \(ACLs\) to manage permissions on files and folders in your file share\. For more information, see [Using Microsoft Windows ACLs to control access to an SMB file share](smb-acl.md)\.

A file gateway can host one or more file shares of different types\. You can have multiple NFS and SMB file shares on a file gateway\.

**Important**  
To create a file share, a file gateway requires you to activate AWS Security Token Service \(AWS STS\)\. Make sure that AWS STS is activated in the AWS Region that you are creating your file gateway in\. If AWS STS is not activated in that AWS Region, activate it\. For information about how to activate AWS STS, see [Activating and deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#sts-regions-activate-deactivate) in the *AWS Identity and Access Management User Guide*\.

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt objects that your file gateway stores in Amazon S3\. To do this using the Storage Gateway console, see [Create an NFS file share](CreatingAnNFSFileShare.md) or [Creating an SMB file share with Active Directory or guest access](CreatingAnSMBFileShare.md#create-SMB-file-share)\. You can also do this by using the Storage Gateway API\. For instructions, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) or [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) in the *Storage Gateway API Reference*\.  
By default, a file gateway uses server\-side encryption managed with Amazon S3 \(SSE\-S3\) when it writes data to an S3 bucket\. If you make SSE\-KMS \(server\-side encryption with AWS KMS–managed keys\) the default encryption for your S3 bucket, objects that a file gateway stores there are encrypted using SSE\-KMS\.  
To encrypt using SSE\-KMS with your own AWS KMS key, you must enable SSE\-KMS encryption\. When you do so, provide the Amazon Resource Name \(ARN\) of the KMS key when you create your file share\. You can also update KMS settings for your file share by using the [UpdateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html) or [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html) API operation\. This update applies to objects stored in the Amazon S3 buckets after the update\.  
If you configure your file gateway to use SSE\-KMS for encryption, you must manually add *kms:Encrypt*, *kms:Decrypt*, *kms:ReEncrypt*, *kms:GenerateDataKey*, and *kms:DescribeKey* permissions to the IAM role associated with the file share\. For more information, see [Using Identity\-Based Policies \(IAM Policies\) for Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/using-identity-based-policies.html)\.

**Topics**
+ [Create an NFS file share](CreatingAnNFSFileShare.md)
+ [Create an SMB file share](CreatingAnSMBFileShare.md)