# Managing Your File Gateway<a name="managing-gateway-file"></a>

Following, you can find information about how to manage your file gateway resources\.

**Topics**
+ [Adding a File Share](#add-file-share)
+ [Deleting a File Share](#remove-file-share)
+ [Updating a File Share](#update-file-share)
+ [Refreshing Objects in Your Amazon S3 Bucket](#refresh-cache)
+ [Understanding File Share Status](#understand-file-share)
+ [File Share Best Practices](#fileshare-best-practices)

## Adding a File Share<a name="add-file-share"></a>

After your file gateway is activated and running, you can add additional file shares and grant access to Amazon S3 buckets\. Buckets that you can grant access to include buckets in a different AWS account than your file share\. For information about how to add a file share, see [Creating a File Share](GettingStartedCreateFileShare.md)\.

**Topics**
+ [Granting Access to an Amazon S3 Bucket](#grant-access-s3)
+ [Using a File Share for Cross\-Account Access](#cross-account-access)

### Granting Access to an Amazon S3 Bucket<a name="grant-access-s3"></a>

When you create a file share, your file gateway requires access to upload files into your Amazon S3 bucket\. To grant this access, your file gateway assumes an AWS Identity and Access Management \(IAM\) role that is associated with an IAM policy that grants this access\. 

The role requires this IAM policy and a security token service trust \(STS\) relationship for it\. The policy determines which actions the role can perform\. In addition, your S3 bucket must have an access policy that allows the IAM role to access the S3 bucket\. 

You can create the role and access policy yourself, or your file gateway can create them for you\. If your file gateway creates the policy for you, the policy contains a list of S3 actions\. For information about roles and permissions, see [Creating a Role to Delegate Permissions to an AWS Service](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

The following example is a trust policy that allows your file gateway to assume an IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "storagegateway.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

If you don’t want your file gateway to create a policy on your behalf, you create your own policy and attach it to your file share\. For more information about how to do this, see [Creating a File Share](GettingStartedCreateFileShare.md)\.

The following example policy allows your file gateway to perform all the Amazon S3 actions listed in the policy\. The first part of the statement allows all the actions listed to be performed on the S3 bucket named `TestBucket`\. The second part allows the listed actions on all objects in `TestBucket`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:GetAccelerateConfiguration",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:ListBucket",
                "s3:ListBucketVersions",
                "s3:ListBucketMultipartUploads"
            ],
            "Resource": "arn:aws:s3:::TestBucket",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:AbortMultipartUpload",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:ListMultipartUploadParts",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::TestBucket/*",
            "Effect": "Allow"
        }
    ]
}
```

### Using a File Share for Cross\-Account Access<a name="cross-account-access"></a>

*Cross\-account* access is when an AWS account and users for that account are granted access to resources that belong to another AWS account\. With file gateways, you can use a file share in one AWS account to access objects in an Amazon S3 bucket that belongs to a different AWS account\. 

**To use a file share owned by one AWS account to access an S3 bucket in a different AWS account**

1. Make sure that the S3 bucket owner has granted your AWS account access to the S3 bucket that you need to access and the objects in that bucket\. For information about how to grant this access, see [Example 2: Bucket Owner Granting Cross\-Account Bucket Permissions](http://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.htm) in the *Amazon Simple Storage Service Developer Guide*\. For a list of the required permissions, see [Granting Access to an Amazon S3 Bucket](#grant-access-s3)\.

1. Make sure that the IAM role that your file share uses to access the S3 bucket includes permissions for operations such as `s3:GetObjectAcl` and `s3:PutObjectAcl`\. In addition, make sure that the IAM role includes a trust policy that allows your account to assume that IAM role\. For an example of such a trust policy, see [Granting Access to an Amazon S3 Bucket](#grant-access-s3)\.

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Give bucket owner full control** in the **Object metadata** settings in the **Configure file share setting** dialog box\. For more information on creating or updating a file share, see [Creating a File Share](GettingStartedCreateFileShare.md) or [Updating a File Share](#update-file-share)\.

When you have created or updated your file share for cross\-account access and mounted the file share on\-premises, we highly recommend that you test your setup\. You can do this by listing directory contents or writing test files and making sure the files show up as objects in the S3 bucket\.

**Important**  
Make sure to set up the policies correctly to grant cross\-account access to the account used by your file share\. If you don't, updates to files through your on\-premises applications don't propagate to the Amazon S3 bucket that you're working with\. 

#### Resources<a name="related-topics-fileshare"></a>

For additional information about access policies and access control lists, see the following:

[Guidelines for Using the Available Access Policy Options](http://docs.aws.amazon.com/AmazonS3/latest/dev/access-policy-alternatives-guidelines.html) in the *Amazon Simple Storage Service Developer Guide*

[Access Control List \(ACL\) Overview](http://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html) in the *Amazon Simple Storage Service Developer Guide*

## Deleting a File Share<a name="remove-file-share"></a>

If you no longer need a file share, you can delete it from the AWS Storage Gateway Management Console\. When you delete a file share, the gateway is detached from the Amazon S3 bucket that the file share maps to\. However, the S3 bucket and its contents aren't deleted\. 

If your gateway is uploading data to a S3 bucket when you delete a file share, the delete process doesn't complete until all the data is uploaded\. The file share has the DELETING status until the data is completely uploaded\.

If you want your data to be completely uploaded, use the **To delete a file share** procedure directly following\. If you don't want to wait for your data to be completely uploaded, see the **To forcibly delete a file share** procedure later in this topic\.

**To delete a file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and choose the file share you want to delete\. 

1. For **Actions**, choose **Delete file share**\. The following confirmation dialog box appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/delete-fileshare.png)

1. In the confirmation dialog box, select the check box for the file share or shares that you want to delete, and then choose **Delete**\. 

In certain cases, you might not want to wait until all the data written to files on the Network File System \(NFS\) file share is uploaded before deleting the file share\. For example, you might want to intentionally discard data that was written but has not yet been uploaded\. In another example, the Amazon S3 bucket or objects that back the file share might have already been deleted, meaning that uploading the specified data is no longer possible\.

In these cases, you can forcibly delete the file share by using the AWS Management Console or the `DeleteFileShare` API operation\. This operation aborts the data upload process\. When it does, the file share enters the FORCE\_DELETING status\. To forcibly delete a file share from the console, see the procedure following\.<a name="force-delete"></a>

**To forcibly delete a file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and choose the file share you want to forcibly delete and wait for a few seconds\. A delete message is displayed in the **Details** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/force-delete3.png)
**Note**  
You cannot undo the force delete operation\.

1. In the message that appears in **Details** tab, verify the ID of the file share you want to forcibly delete, select the confirmation box, and choose **Force delete now**\.

You can also use the [DeleteFileShare](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html) API operation to forcibly delete the file share\.

## Updating a File Share<a name="update-file-share"></a>

You can update the default file share settings, the clients allowed to connect to your file share, and the metadata defaults for your file share\. 

### Editing the File Share Settings<a name="edit-storage-class"></a>

You can edit the default storage class for your Amazon S3 bucket, the squash level setting, and the **Export as** option for your file share\. Possible **Export as** options include, for example, **Read\-write**\.

**To edit the file share settings**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share that you want to update\. 

1. For **Actions**, choose **Edit file share settings**\.

1. Do one or more of the following:
   + For **Storage class for new objects**, choose a default storage class for your S3 bucket, and choose **Save**\.

     Possible values for the storage class for new objects are the following:
     + **S3 Standard **– Store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
     + **S3 Standard\_IA **– Store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
     + **S3 One Zone\_IA** – Store your infrequently accessed object data a single Availability Zone\.

       For more information, see [Storage Classes](http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro) in the *Amazon Simple Storage Service Developer Guide*\.
   + For **Object metadata**, choose the metadata you want to use:
     + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
     + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file NFS file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](#cross-account-access)\.
     + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.
   + For **Squash level**, choose the squash level setting you want for your file share, and then choose **Save**\. Possible values are the following:
     + **Root squash \(default\)** – Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.
     + **No root squash** – The remote superuser \(root\) receives access as root\.
     + **All squash – **All user access is mapped to UID \(65534\) and GID \(65534\)\.

     The default value for squash level is **Root squash**\.
   + For **Export as**, choose an option for your file share, and then choose **Save**\. The default value is **Read\-write**\.
**Note**  
For file shares mounted on a Microsoft Windows client, if you select **Read\-only** for **Export as**, you might see an error message about an unexpected error keeping you from creating the folder\. This error message is a known issue with NFS version 3\. You can ignore the message\.

### Editing Metadata Defaults<a name="edit-metadata-defaults"></a>

If you don't set metadata values for your files or directories in your bucket, your file gateway sets default metadata values\. These values include Unix permissions for files and folders\. You can edit the metadata defaults on the AWS Storage Gateway Management Console\. 

When your file gateway stores files and folders in Amazon S3, the Unix file permissions are stored in object metadata\. When your file gateway discovers objects that weren't stored by the file gateway, these objects are assigned default Unix file permissions\. You can find the default Unix permissions in the following table\.


| Metadata | Description | 
| --- | --- | 
| Directory permissions |  The Unix directory mode in the form "nnnn"\. For example, "0666" represents the access mode for all directories inside the file share\. The default value is 0777\.  | 
| File permissions |  The Unix file mode in the form "nnnn"\. For example, "0666" represents the file mode inside the file share\. The default value is 0666\.   | 
| User ID |  The default owner ID for files in the file share\. The default value is 65534\.   | 
| Group ID | The default group ID for the file share\. The default value is 65534\.  | 

**To edit metadata defaults**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share you want to update\. 

1. For **Actions**, choose **Edit file metadata defaults**\.

1. In the **Edit file metadata defaults** dialog box, provide the metadata information and choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/edit-metadata-defaults.png)

### Editing Allowed NFS Clients<a name="edit-nfs-client"></a>

We recommend changing the allowed NFS client settings for your file share\. If you don't, any client on your network can mount to your file share\.

**To edit allowed NFS clients**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share you want to update\. 

1. For **Actions**, choose **Edit allowed clients**\.

1. In the **Edit allowed clients** dialog box, choose **Add entry**, provide the IP address or CIDR for the client, and then choose **Save**\.

## Refreshing Objects in Your Amazon S3 Bucket<a name="refresh-cache"></a>

As your NFS client performs file system operations, your gateway maintains an inventory of the objects in the Amazon S3 bucket associated with your file share\. Your gateway uses this cached inventory to reduce the latency and frequency of S3 requests\. 

To refresh the S3 bucket for your file share, you can use the AWS Storage Gateway console or the [RefreshCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) operation in the AWS Storage Gateway API\.

**To refresh objects in a S3 bucket from the console**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share associated with the S3 bucket that you want to refresh\. 

1. For **Actions**, choose **Refresh cache**\. The time that it takes to refresh depends on the number of objects that the S3 bucket contains\.

## Understanding File Share Status<a name="understand-file-share"></a>

Each file share has an associated status that tells you at a glance what the health of the file share is\. Most of the time, the status indicates that the file share is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem that might or might not require action on your part\.

You can see file share status on the AWS Storage Gateway console\. File share status appears in the **Status** column for each file share in your gateway\. A file share that is functioning normally has the status of AVAILABLE\. 

In the following table, you can find a description of each file share status, and if and when you should act based on the status\. A file share should have AVAILABLE status all or most of the time it's in use\. 


| Status | Meaning | 
| --- | --- | 
| AVAILABLE |  The file share is configured properly and is available to use\. The AVAILABLE status is the normal running status for a file share\.  | 
| CREATING |  The file share is being created and is not ready for use\. The CREATING status is transitional\. No action is required\. If file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| UPDATING |  The file share configuration is being updated\. If a file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| DELETING |  The file share is being deleted\. The file share is not deleted until all data is uploaded to AWS\. The DELETING status is transitional, and no action is required\.  | 
| FORCE\_DELETING |  The file share is being deleted forcibly\. The file share is deleted immediately and uploading to AWS is aborted\. The FORCE\_DELETING status is transitional, and no action is required\.  | 
| UNAVAILABLE |  The file share is in an unhealthy state\. Certain issues can cause the file share to go into an unhealthy state\. For example, role policy errors can cause this, or if the file share maps to a Amazon S3 bucket that doesn't exist\. When the issue that caused the unhealthy state is resolved, the file returns to AVAILABLE state\.  | 

## File Share Best Practices<a name="fileshare-best-practices"></a>

In this section, you can find information about best practices for creating file shares\.

**Topics**
+ [Preventing Multiple File Shares Writing to Your Amazon S3 Bucket](#prevent-multiple-writes)
+ [Allowing Specific NFS Clients to Mount Your File Share](#nfs-client-mount)

### Preventing Multiple File Shares Writing to Your Amazon S3 Bucket<a name="prevent-multiple-writes"></a>

When you create a file share, we recommend that you configure your Amazon S3 bucket so that only one file share can write to it\. If you configure your S3 bucket to be written to by multiple file shares, unpredictable results can occur\. To prevent this, create an S3 bucket policy that denies all roles except the role used for the file share to put or delete objects in the bucket\. Then attach this policy to the S3 bucket\.

The following example policy denies all roles except the role that created the bucket to write to the S3 bucket\. The `s3:DeleteObject` and `s3:PutObject` actions are denied for all roles except `"TestUser"`\. The policy applies to all objects in the `"arn:aws:s3:::test-bucket/*"` bucket\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"DenyMultiWrite",
         "Effect":"Deny",
         "Principal":"*",
         "Action":[
            "s3:DeleteObject",
            "s3:PutObject"
         ],
         "Resource":"arn:aws:s3:::TestBucket/*",
         "Condition":{
            "StringNotLike":{
               "aws:userid":"TestUser:*"
            }
         }
      }
   ]
}
```

### Allowing Specific NFS Clients to Mount Your File Share<a name="nfs-client-mount"></a>

We recommend that you change the allowed NFS client settings for your file share\. If you don't, any client on your network can mount your file share\. For information about how to edit your NFS client settings, see [Editing Allowed NFS Clients](#edit-nfs-client)\.