# Troubleshooting file share issues<a name="troubleshooting-file-share-issues"></a>

You can find information following about actions to take if you experience unexpected issues with your file share\.

**Topics**
+ [Your file share is stuck in CREATING status](#creating-state)
+ [You can't create a file share](#create-file-troubleshoot)
+ [SMB file shares don't allow multiple different access methods](#smb-fileshare-troubleshoot)
+ [Multiple file shares can't write to the mapped S3 bucket](#multiwrite)
+ [Can't upload files into your S3 bucket](#access-s3bucket)
+ [Can't change the default encryption to use SSE\-KMS to encrypt objects stored in my S3 bucket](#encryption-issues)
+ [Changes made directly in an S3 bucket with object versioning enabled may affect what you see in your file share](#s3-object-versioning-file-share-issue)
+ [When writing to an S3 bucket with object versioning enabled, the file gateway may create multiple versions of an S3 object](#s3-object-versioning-file-gateway-issue)
+ [ACL permissions aren't working as expected](#smb-acl-issues)
+ [Your gateway performance declined after you performed a recursive operation](#recursive-operation-issues)

## Your file share is stuck in CREATING status<a name="creating-state"></a>

When your file share is being created, the status is CREATING\. The status transitions to AVAILABLE status after the file share is created\. If your file share gets stuck in the CREATING status, do the following:

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Make sure the S3 bucket that you mapped your file share to exists\. If the bucket doesn’t exist, create it\. After you create the bucket, the file share status transitions to AVAILABLE\. For information about how to create an S3 bucket, see [Create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Make sure your bucket name complies with the rules for bucket naming in Amazon S3\. For more information, see [Rules for bucket naming](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.

1. Make sure the IAM role you used to access the S3 bucket has the correct permissions and verify that the S3 bucket is listed as a resource in the IAM policy\. For more information, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

## You can't create a file share<a name="create-file-troubleshoot"></a>

1. If you can't create a file share because your file share is stuck in CREATING status, verify that the S3 bucket you mapped your file share to exists\. For information on how to do so, see [Your file share is stuck in CREATING status](#creating-state), preceding\.

1. If the S3 bucket exists, then verify that AWS Security Token Service is enabled in the region where you are creating the file share\. If a security token is not enabled, you should enable it\. For information about how to enable a token using AWS Security Token Service, see [Activating and deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

## SMB file shares don't allow multiple different access methods<a name="smb-fileshare-troubleshoot"></a>

SMB file shares have the following restrictions:

1. When the same client attempts to mount both an Active Directory and Guest access SMB file share the following error message is displayed: `Multiple connections to a server or shared resource by the same user, using more than one user name, are not allowed. Disconnect all previous connections to the server or shared resource and try again.`

1. A Windows user cannot remain connected to two Guest Access SMB file shares, and may be disconnected when a new Guest Access connection is established\.

1. A Windows client can't mount both a Guest Access and an Active Directory SMB file share that is exported by the same gateway\.

## Multiple file shares can't write to the mapped S3 bucket<a name="multiwrite"></a>

We don't recommend configuring your S3 bucket to allow multiple file shares to write to one S3 bucket\. This approach can cause unpredictable results\. 

Instead, we recommend that you allow only one file share to write to each S3 bucket\. You create a bucket policy to allow only the role associated with your file share to write to the bucket\. For more information, see [File share best practices](managing-gateway-file.md#fileshare-best-practices)\.

## Can't upload files into your S3 bucket<a name="access-s3bucket"></a>

If you can't upload files into your S3 bucket, do the following:

1. Make sure you have granted the required access for the file gateway to upload files into your S3 bucket\. For more information, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. Make sure the role that created the bucket has permission to write to the S3 bucket\. For more information, see [File share best practices](managing-gateway-file.md#fileshare-best-practices)\.

## Can't change the default encryption to use SSE\-KMS to encrypt objects stored in my S3 bucket<a name="encryption-issues"></a>

If you change the default encryption and make SSE\-KMS \(server\-side encryption with AWS KMS–managed keys\) the default for your S3 bucket, objects that a file gateway stores in the bucket are not encrypted with SSE\-KMS\. By default, a file gateway uses server\-side encryption managed with Amazon S3 \(SSE\-S3\) when it writes data to an S3 bucket\. Changing the default won't automatically change your encryption\.

To change the encryption to use SSE\-KMS with your own AWS KMS key, you must enable SSE\-KMS encryption\. To do so, you provide the Amazon Resource Name \(ARN\) of the KMS key when you create your file share\. You can also update KMS settings for your file share by using the `UpdateNFSFileShare` or `UpdateSMBFileShare` API operation\. This update applies to objects stored in the S3 buckets after the update\. For more information, see [Data Encryption Using AWS KMS](encryption.md)\.

## Changes made directly in an S3 bucket with object versioning enabled may affect what you see in your file share<a name="s3-object-versioning-file-share-issue"></a>

If your S3 bucket has objects written to it by another client, your view of the S3 bucket might not be up\-to\-date as a result of S3 bucket object versioning\. You should always refresh your cache before examining files of interest\.

*Object versioning *is an optional S3 bucket feature that helps protect data by storing multiple copies of the same\-named object\. Each copy has a separate ID value, for example `file1.jpg`: `ID="xxx"` and `file1.jpg`: `ID="yyy"`\. The number of identically named objects and their lifetimes is controlled by Amazon S3 lifecycle policies\. For more details on these Amazon S3 concepts, see [Using versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html) and [Object lifecycle management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) in the *Amazon S3 Developer Guide\. * 

When you delete a versioned object, that object is flagged with a delete marker but retained\. Only an S3 bucket owner can permanently delete an object with versioning turned on\.

In your file gateway, files shown are the most recent versions of objects in an S3 bucket at the time the object was fetched or the cache was refreshed\. File gateways ignore any older versions or any objects marked for deletion\. When reading a file, you read data from the latest version\. When you write a file in your file share, your file gateway creates a new version of a named object with your changes, and that version becomes the latest version\.

Your file gateway continues to read from the earlier version, and updates that you make are based on the earlier version should a new version be added to the S3 bucket outside of your application\. To read the latest version of an object, use the [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) API action or refresh from the console as described in [Refreshing objects in your Amazon S3 bucket](managing-gateway-file.md#refresh-cache)\.

**Important**  
We don't recommend that objects or files be written to your file gateway S3 bucket from outside of the file share\.

## When writing to an S3 bucket with object versioning enabled, the file gateway may create multiple versions of an S3 object<a name="s3-object-versioning-file-gateway-issue"></a>

With object versioning enabled, you may have multiple versions of an object created in Amazon S3 on every update to a file from your NFS or SMB client\. Here are scenarios that can result in multiple versions of an object being created in your S3 bucket:
+ When a file is modified in the file gateway by an NFS or SMB client after it has been uploaded to Amazon S3, the file gateway uploads the new or modified data instead of uploading the whole file\. The file modification results in a new version of the Amazon S3 object being created\.
+ When a file is written to the file gateway by an NFS or SMB client, the file gateway uploads the file's data to Amazon S3 followed by its metadata, \(ownerships, timestamps, etc\.\)\. Uploading the file data creates an Amazon S3 object, and uploading the metadata for the file updates the metadata for the Amazon S3 object\. This process creates another version of the object, resulting in two versions of an object\.
+ When the file gateway is uploading larger files, it might need to upload smaller chunks of the file before the client is done writing to the file gateway\. Some reasons for this include to free up cache space or a high rate of writes to a file\. This can result in multiple versions of an object in the S3 bucket\.

You should monitor your S3 bucket to determine how many versions of an object exist before setting up lifecycle policies to move objects to different storage classes\. You should configure lifecycle expiration for previous versions to minimize the number of versions you have for an object in your S3 bucket\. The use of Same\-Region replication \(SRR\) or Cross\-Region replication \(CRR\) between S3 buckets will increase the storage used\. For more information about replication, see [Replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html)\.

**Important**  
Do not configure replication between S3 buckets until you understand how much storage is being used when object versioning is enabled\.

Use of versioned S3 buckets can greatly increase the amount of storage in Amazon S3 because each modification to a file creates a new version of the S3 object\. By default, Amazon S3 continues to store all of these versions unless you specifically create a policy to override this behavior and limit the number of versions that are kept\. If you notice unusually large storage usage with object versioning enabled, check that you have your storage policies set appropriately\. An increase in the number of `HTTP 503-slow down` responses for browser requests can also be the result of problems with object versioning\.

If you enable object versioning after installing a file gateway, all unique objects are retained \(`ID=”NULL”`\) and you can see them all in the file system\. New versions of objects are assigned a unique ID \(older versions are retained\)\. Based on the object's timestamp only the newest versioned object is viewable in the NFS file system\.

After you enable object versioning, your S3 bucket can't be returned to a nonversioned state\. You can, however, suspend versioning\. When you suspend versioning, a new object is assigned an ID\. If the same named object exists with an `ID=”NULL”` value, the older version is overwritten\. However, any version that contains a non\-`NULL` ID is retained\. Timestamps identify the new object as the current one, and that is the one that appears in the NFS file system\.

## ACL permissions aren't working as expected<a name="smb-acl-issues"></a>

If access control list \(ACL\) permissions aren't working as you expect with your SMB file share, you can perform a test\. 

To do this, first test the permissions on a Microsoft Windows file server or a local Windows file share\. Then compare the behavior to your gateway's file share\.

## Your gateway performance declined after you performed a recursive operation<a name="recursive-operation-issues"></a>

In some cases, you might perform a recursive operation, such as renaming a directory or enabling inheritance for an ACL, and force it down the tree\. If you do this, your file gateway recursively applies the operation to all objects in the file share\. 

For example, suppose that you apply inheritance to existing objects in an S3 bucket\. Your file gateway recursively applies inheritance to all objects in the bucket\. Such operations can cause your gateway performance to decline\.