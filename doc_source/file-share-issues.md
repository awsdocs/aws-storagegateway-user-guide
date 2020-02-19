# Troubleshooting File Share Issues<a name="file-share-issues"></a>

You can find information following about actions to take if you experience unexpected issues with your file share\.

**Topics**
+ [Your File Share Is Stuck in CREATING Status](#creating-state)
+ [You Can't Create a File Share](#create-file-troubleshoot)
+ [SMB File Shares Do Not Allow Multiple Different Access Methods](#smb-fileshare-troubleshoot)
+ [Multiple File Shares Can't Write to the Mapped Amazon S3 Bucket](#multiwrite)
+ [You Can't Upload Files into Your S3 Bucket](#access-s3bucket)
+ [Can't Change the Default Encryption to Use SSE\-KMS to Encrypt Objects Stored in My Amazon S3 Bucket\.](#encryption-issues)
+ [Object Versioning Might Affect What You See in Your File System](#swg-object-versioning)
+ [ACL Permissions Aren't Working as Expected](#smb-acl-issues)
+ [Your Gateway Performance Declined After You Performed a Recursive Operation](#recursive-operation-issues)

## Your File Share Is Stuck in CREATING Status<a name="creating-state"></a>

When your file share is being created, the status is CREATING\. The status transitions to AVAILABLE status after the file share is created\. If your file share gets stuck in the CREATING status, do the following:

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Make sure the Amazon S3 bucket that you mapped your file share to exists\. If the bucket doesn’t exist, create it\. After you create the bucket, the file share status transitions to AVAILABLE\. For information about how to create an Amazon S3 bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Make sure your bucket name complies with the rules for bucket naming in Amazon S3\. For more information, see [Rules for Bucket Naming](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.

1. Make sure the IAM role you used to access the Amazon S3 bucket has the correct permissions and verify that the Amazon S3 bucket is listed as a resource in the IAM policy\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\.

## You Can't Create a File Share<a name="create-file-troubleshoot"></a>

1. If you can't create a file share because your file share is stuck in CREATING status, verify that the Amazon S3 bucket you mapped your file share to exists\. For information on how to do so, see [Your File Share Is Stuck in CREATING Status](#creating-state), preceding\.

1. If the Amazon S3 bucket exists, then verify that AWS Security Token Service is enabled in the region where you are creating the file share\. If a security token is not enabled, you should enable it\. For information about how to enable a token using AWS Security Token Service, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

## SMB File Shares Do Not Allow Multiple Different Access Methods<a name="smb-fileshare-troubleshoot"></a>

SMB file shares have the following restrictions:

1. When the same client attempts to mount both an Active Directory and Guest access SMB file share the following error message is displayed: `Multiple connections to a server or shared resource by the same user, using more than one user name, are not allowed. Disconnect all previous connections to the server or shared resource and try again.`

1. A Windows user cannot remain connected to two Guest Access SMB file shares, and may be disconnected when a new Guest Access connection is established\.

1. A Windows client can't mount both a Guest Access and an Active Directory SMB file share that is exported by the same gateway\.

## Multiple File Shares Can't Write to the Mapped Amazon S3 Bucket<a name="multiwrite"></a>

We don't recommend configuring your Amazon S3 bucket to allow multiple file shares to write to one S3 bucket\. This approach can cause unpredictable results\. 

Instead, we recommend that you allow only one file share to write to each S3 bucket\. You create a bucket policy to allow only the role associated with your file share to write to the bucket\. For more information, see [File Share Best Practices](managing-gateway-file.md#fileshare-best-practices)\.

## You Can't Upload Files into Your S3 Bucket<a name="access-s3bucket"></a>

If you can't upload files into your Amazon S3 bucket, do the following:

1. Make sure you have granted the required access for the file gateway to upload files into your S3 bucket\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\.

1. Make sure the role that created the bucket has permission to write to the S3 bucket\. For more information, see [File Share Best Practices](managing-gateway-file.md#fileshare-best-practices)\.

## Can't Change the Default Encryption to Use SSE\-KMS to Encrypt Objects Stored in My Amazon S3 Bucket\.<a name="encryption-issues"></a>

If you change the default encryption and make SSE\-KMS \(server\-side encryption with AWS KMS–managed keys\) the default for your S3 bucket, objects that a file gateway stores in the bucket are not encrypted with SSE\-KMS\. By default, a file gateway uses server\-side encryption managed with Amazon S3 \(SSE\-S3\) when it writes data to an Amazon S3 bucket\. Changing the default won't automatically change your encryption\.

To change the encryption to use SSE\-KMS with your own AWS KMS key, you must enable SSE\-KMS encryption\. To do so, you provide the Amazon Resource Name \(ARN\) of the KMS key when you create your file share\. You can also update KMS settings for your file share by using the `UpdateNFSFileShare` or `UpdateSMBFileShare` API operation\. This update applies to objects stored in the Amazon S3 buckets after the update\. For more information, see [Data Encryption Using AWS KMS](encryption.md)\.

## Object Versioning Might Affect What You See in Your File System<a name="swg-object-versioning"></a>

If your Amazon S3 bucket has objects written to it by another client, your view of the S3 bucket might not be up\-to\-date as a result of S3 bucket object versioning\. You should always refresh your cache before examining files of interest\.

*Object versioning *is an optional S3 bucket feature that helps protect data by storing multiple copies of the same\-named object\. Each copy has a separate ID value, for example `file1.jpg`: `ID="xxx"` and `file1.jpg`: `ID="yyy"`\. The number of identically named objects and their lifetimes is controlled by S3 lifecycle policies\. For more details on these S3 concepts, see [Using Versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html) and [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) in the *Amazon S3 Developer Guide\. * 

When you delete a versioned object, that object is flagged with a delete marker but retained\. Only an S3 bucket owner can permanently delete an object with versioning turned on\.

In your file gateway, files shown are the most recent versions of objects in an S3 bucket at the time the object was fetched or the cache was refreshed\. File gateways ignore any older versions or any objects marked for deletion\. When reading a file, you read data from the latest version\. When you write a file in your file share, your file gateway creates a new version of a named object with your changes, and that version becomes the latest version\.

Your file gateway continues to read from the earlier version, and updates that you make are based on the earlier version should a new version be added to the S3 bucket outside of your application\. To read the latest version of an object, use the [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) API action or refresh from the console as described in [Refreshing Objects in Your Amazon S3 Bucket](managing-gateway-file.md#refresh-cache)\. We don't recommend that objects or files be written to your file gateway S3 bucket from outside of the file share\.

Use of versioned S3 buckets can greatly increase the amount of storage in S3 because each modification to a file creates a new version\. By default, S3 continues to store all of these versions unless you specifically create a policy to override this behavior and limit the number of versions that are kept\. If you notice unusually large storage usage with object versioning enabled, check that you have your storage policies set appropriately\. An increase in the number of `HTTP 503-slow down` responses for browser requests can also be the result of problems with object versioning\.

If you enable object versioning after installing a file gateway, all unique objects are retained \(`ID=”NULL”`\) and you can see them all in the file system\. New versions of objects are assigned a unique ID \(older versions are retained\)\. Based on the object's timestamp only the newest versioned object is viewable in the NFS file system\.

After you enable object versioning, your S3 bucket can't be returned to a nonversioned state\. You can, however, suspend versioning\. When you suspend versioning, a new object is assigned an ID\. If the same named object exists with an `ID=”NULL”` value, the older version is overwritten\. However, any version that contains a non\-`NULL` ID is retained\. Timestamps identify the new object as the current one, and that is the one that appears in the NFS file system\.

## ACL Permissions Aren't Working as Expected<a name="smb-acl-issues"></a>

If access control list \(ACL\) permissions aren't working as you expect with your SMB file share, you can perform a test\. 

To do this, first test the permissions on a Microsoft Windows file server or a local Windows file share\. Then compare the behavior to your gateway's file share\.

## Your Gateway Performance Declined After You Performed a Recursive Operation<a name="recursive-operation-issues"></a>

In some cases, you might perform a recursive operation, such as renaming a directory or enabling inheritance for an ACL, and force it down the tree\. If you do this, your file gateway recursively applies the operation to all objects in the file share\. 

For example, suppose that you apply inheritance to existing objects in an Amazon S3 bucket\. Your file gateway recursively applies inheritance to all objects in the bucket\. Such operations can cause your gateway performance to decline\.