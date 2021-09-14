# Creating a file share<a name="GettingStartedCreateFileShare"></a>

In this section, you can find instructions about how to create a file share\. You can create a file share that can be accessed using either the Network File System \(NFS\) or Server Message Block \(SMB\) protocol\.

When you create an NFS share, by default anyone who has access to the NFS server can access the NFS file share\. You can limit access to clients by IP address\.

For SMB, you can have one of three different modes of authentication:
+ A file share with Microsoft Active Directory \(AD\) access\. Any authenticated Microsoft AD user gets access to this file share type\.
+ An SMB file share with limited access\. Only certain domain users and groups that you specify are allowed access \(allow list\)\. Users and groups can also be denied access \(deny list\)\.
+ An SMB file share with guest access\. Any users who can provide the guest password get access to this file share\.
**Note**  
File shares exported through the gateway for NFS file shares support POSIX permissions\. For SMB file shares, you can use access control lists \(ACLs\) to manage permissions on files and folders in your file share\. For more information, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.

A file gateway can host one or more file shares of different types\. You can have multiple NFS and SMB file shares on a file gateway\.

**Important**  
To create a file share, a file gateway requires you to activate AWS Security Token Service \(AWS STS\)\. Make sure that AWS STS is activated in the AWS Region that you are creating your file gateway in\. If AWS STS is not activated in that AWS Region, activate it\. For information about how to activate AWS STS, see [Activating and deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#sts-regions-activate-deactivate) in the *AWS Identity and Access Management User Guide*\.

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt objects that your file gateway stores in Amazon S3\. To do this using the Storage Gateway console, see [Creating an NFS file share](CreatingAnNFSFileShare.md) or [Creating an SMB file share](CreatingAnSMBFileShare.md#create-SMB-file-share)\. You can also do this by using the Storage Gateway API\. For instructions, see the [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) or [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) in the AWS Storage Gateway API Reference\.  
By default, a file gateway uses server\-side encryption managed with Amazon S3 \(SSE\-S3\) when it writes data to an S3 bucket\. If you make SSE\-KMS \(server\-side encryption with AWS KMS–managed keys\) the default encryption for your S3 bucket, objects that a file gateway stores there are encrypted using SSE\-KMS\.  
To encrypt using SSE\-KMS with your own AWS KMS key, you must enable SSE\-KMS encryption\. When you do so, provide the Amazon Resource Name \(ARN\) of the KMS key when you create your file share\. You can also update KMS settings for your file share by using the [UpdateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html) or [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html) API operation\. This update applies to objects stored in the Amazon S3 buckets after the update\.

**Topics**
+ [Creating an NFS file share](CreatingAnNFSFileShare.md)
+ [Creating an SMB file share](CreatingAnSMBFileShare.md)