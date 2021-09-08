# Managing your Amazon S3 File Gateway<a name="managing-gateway-file"></a>

Following, you can find information about how to manage your Amazon S3 File Gateway resources\.

**Topics**
+ [Adding a file share](#add-file-share)
+ [Deleting a file share](#remove-file-share)
+ [Editing settings for your NFS file share](#edit-storage-class)
+ [Editing metadata defaults for your NFS file share](#edit-metadata-defaults)
+ [Editing access settings for your NFS file share](#edit-nfs-client)
+ [Editing gateway level access settings for your SMB file share](#edit-smb-access-settings)
+ [Editing settings for your SMB file share](#edit-smbfileshare-settings)
+ [Refreshing objects in your Amazon S3 bucket](#refresh-cache)
+ [Using S3 object lock with a Amazon S3 File Gateway](#s3-object-lock)
+ [Understanding file share status](#understand-file-share)
+ [File share best practices](#fileshare-best-practices)

## Adding a file share<a name="add-file-share"></a>

After your S3 File is activated and running, you can add additional file shares and grant access to Amazon S3 buckets\. Buckets that you can grant access to include buckets in a different AWS account than your file share\. For information about how to add a file share, see \.

**Topics**
+ [Granting access to an Amazon S3 bucket](#grant-access-s3)
+ [Using a file share for cross\-account access](#cross-account-access)

### Granting access to an Amazon S3 bucket<a name="grant-access-s3"></a>

When you create a file share, your file gateway requires access to upload files into your Amazon S3 bucket, and to perform actions on any access points or VPC endpoints that it uses to connect to the bucket\. To grant this access, your file gateway assumes an AWS Identity and Access Management \(IAM\) role that is associated with an IAM policy that grants this access\.

The role requires this IAM policy and a security token service trust \(STS\) relationship for it\. The policy determines which actions the role can perform\. In addition, your S3 bucket and any associated access points or VPC endpoints must have an access policy that allows the IAM role to access them\.

You can create the role and access policy yourself, or your file gateway can create them for you\. If your file gateway creates the policy for you, the policy contains a list of S3 actions\. For information about roles and permissions, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

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

If you don't want your file gateway to create a policy on your behalf, you can create your own policy and attach it to your file share\. For more information about how to do this, see [Create a file share](GettingStartedCreateFileShare.md)\.

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

The following example policy is similar to the one above, but allows your file gateway to perform actions required to access a bucket through an access point\. You can also use this type of role policy if your gateway connects to a file share using an access point alias\. Create the role policy to allow actions on the access point associated with the alias you use\.

```
{
    "Version": "2012-10-17",
    "Statement": [
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
            "Resource": "arn:aws:s3:us-east-1:123456789:accesspoint/TestAccessPointName/*",
            "Effect": "Allow"
        }
    ]
}
```

**Note**  
If you need to connect your file share to an S3 bucket through a VPC endpoint, see [Endpoint policies for Amazon S3](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html#vpc-endpoints-policies-s3) in the *AWS PrivateLink User Guide*\.

### Using a file share for cross\-account access<a name="cross-account-access"></a>

*Cross\-account* access is when an Amazon Web Services account and users for that account are granted access to resources that belong to another Amazon Web Services account\. With file gateways, you can use a file share in one Amazon Web Services account to access objects in an Amazon S3 bucket that belongs to a different Amazon Web Services account\.

**To use a file share owned by one Amazon Web Services account to access an S3 bucket in a different Amazon Web Services account**

1. Make sure that the S3 bucket owner has granted your Amazon Web Services account access to the S3 bucket that you need to access and the objects in that bucket\. For information about how to grant this access, see [Example 2: Bucket owner granting cross\-account bucket permissions](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.html) in the *Amazon Simple Storage Service Developer Guide*\. For a list of the required permissions, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

1. Make sure that the IAM role that your file share uses to access the S3 bucket includes permissions for operations such as `s3:GetObjectAcl` and `s3:PutObjectAcl`\. In addition, make sure that the IAM role includes a trust policy that allows your account to assume that IAM role\. For an example of such a trust policy, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

   If your file share uses an existing role to access the S3 bucket, you should include permissions for `s3:GetObjectAc`l and `s3:PutObjectAcl` operations\. The role also needs a trust policy that allows your account to assume this role\. For an example of such a trust policy, see [Granting access to an Amazon S3 bucket](#grant-access-s3)\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Give bucket owner full control** in the **Object metadata** settings in the **Configure file share setting** dialog box\.

When you have created or updated your file share for cross\-account access and mounted the file share on\-premises, we highly recommend that you test your setup\. You can do this by listing directory contents or writing test files and making sure the files show up as objects in the S3 bucket\.

**Important**  
Make sure to set up the policies correctly to grant cross\-account access to the account used by your file share\. If you don't, updates to files through your on\-premises applications don't propagate to the Amazon S3 bucket that you're working with\.

#### Resources<a name="related-topics-fileshare"></a>

For additional information about access policies and access control lists, see the following:

[Guidelines for using the available access policy options](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-policy-alternatives-guidelines.html) in the *Amazon Simple Storage Service Developer Guide*

[Access Control List \(ACL\) overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html) in the *Amazon Simple Storage Service Developer Guide*

## Deleting a file share<a name="remove-file-share"></a>

If you no longer need a file share, you can delete it from the Storage Gateway Management Console\. When you delete a file share, the gateway is detached from the Amazon S3 bucket that the file share maps to\. However, the S3 bucket and its contents aren't deleted\.

If your gateway is uploading data to a S3 bucket when you delete a file share, the delete process doesn't complete until all the data is uploaded\. The file share has the DELETING status until the data is completely uploaded\.

If you want your data to be completely uploaded, use the **To delete a file share** procedure directly following\. If you don't want to wait for your data to be completely uploaded, see the **To forcibly delete a file share** procedure later in this topic\.

**To delete a file share**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and choose the file share that you want to delete\.

1. For **Actions**, choose **Delete file share**\. The following confirmation dialog box appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/delete-fileshare.png)

1. In the confirmation dialog box, select the check box for the file share or shares that you want to delete, and then choose **Delete**\. 

In certain cases, you might not want to wait until all the data written to files on the Network File System \(NFS\) file share is uploaded before deleting the file share\. For example, you might want to intentionally discard data that was written but has not yet been uploaded\. In another example, the Amazon S3 bucket or objects that back the file share might have already been deleted, meaning that uploading the specified data is no longer possible\.

In these cases, you can forcibly delete the file share by using the AWS Management Console or the `DeleteFileShare` API operation\. This operation aborts the data upload process\. When it does, the file share enters the FORCE\_DELETING status\. To forcibly delete a file share from the console, see the procedure following\.<a name="force-delete"></a>

**To forcibly delete a file share**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and choose the file share that you want to forcibly delete and wait for a few seconds\. A delete message is displayed in the **Details** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/force-delete3.png)
**Note**  
You cannot undo the force delete operation\.

1. In the message that appears in the **Details** tab, verify the ID of the file share that you want to forcibly delete, select the confirmation box, and choose **Force delete now**\.

You can also use the [DeleteFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html) API operation to forcibly delete the file share\.

## Editing settings for your NFS file share<a name="edit-storage-class"></a>

You can edit the storage class for your Amazon S3 bucket, file share name, object metadata, squash level, export as, and automated cache refresh settings\.

**Note**  
You cannot edit an existing file share to point to a new bucket or access point, or to modify the VPC endpoint settings\. You can configure those settings only when creating a new file share\.

**To edit the file share settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share that you want to update\.

1. For **Actions**, choose **Edit share settings**\.

1. Do one or more of the following:
   + For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
     + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + \(Optional\) For **File share name**, enter a new name for the file share\.
   + For **Object metadata**, choose the metadata that you want to use:
     + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
     + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file's Network File System \(NFS\) or Server Message Block \(SMB\) file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](#cross-account-access)\.
     + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.
   + For **Export as**, choose an option for your file share\. The default value is **Read\-write**\.
**Note**  
For file shares mounted on a Microsoft Windows client, if you select **Read\-only** for **Export as**, you might see an error message about an unexpected error keeping you from creating the folder\. This error message is a known issue with NFS version 3\. You can ignore the message\.
   + For **Squash level**, choose the squash level setting that you want for your NFS file share, and then choose **Save**\.
**Note**  
You can choose a squash level setting for NFS file shares only\. SMB file shares don't use squash settings\.

     Possible values are the following:
     + **Root squash \(default\)** – Access for the remote superuser \(root\) is mapped to UID \(65534\) and GID \(65534\)\.
     + **No root squash** – The remote superuser \(root\) receives access as root\.
     + **All squash – **All user access is mapped to UID \(65534\) and GID \(65534\)\.

     The default value for squash level is **Root squash**\.
   + \(Optional\) For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.
   + \(Optional\) For **File upload notification**, choose the check box to be notified when a file has been fully uploaded to S3 by the S3 File\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see \.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. Choose **Save**\.

## Editing metadata defaults for your NFS file share<a name="edit-metadata-defaults"></a>

If you don't set metadata values for your files or directories in your bucket, your S3 File sets default metadata values\. These values include Unix permissions for files and folders\. You can edit the metadata defaults on the Storage Gateway Management Console\.

When your S3 File stores files and folders in Amazon S3, the Unix file permissions are stored in object metadata\. When your S3 File discovers objects that weren't stored by the S3 File, these objects are assigned default Unix file permissions\. You can find the default Unix permissions in the following table\.


| Metadata | Description | 
| --- | --- | 
| Directory permissions |  The Unix directory mode in the form "nnnn"\. For example, "0666" represents the access mode for all directories inside the file share\. The default value is 0777\.  | 
| File permissions |  The Unix file mode in the form "nnnn"\. For example, "0666" represents the file mode inside the file share\. The default value is 0666\.  | 
| User ID |  The default owner ID for files in the file share\. The default value is 65534\.  | 
| Group ID | The default group ID for the file share\. The default value is 65534\.  | 

**To edit metadata defaults**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share that you want to update\.

1. For **Actions**, choose **Edit file metadata defaults**\.

1. In the **Edit file metadata defaults** dialog box, provide the metadata information and choose **Save**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/edit-metadata-defaults.png)

## Editing access settings for your NFS file share<a name="edit-nfs-client"></a>

We recommend changing the allowed NFS client settings for your NFS file share\. If you don't, any client on your network can mount to your file share\.

**To edit NFS access settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the NFS file share that you want to edit\.

1. For **Actions**, choose **Edit share access settings**\.

1. In the **Edit allowed clients** dialog box, choose **Add entry**, provide the IP address or CIDR notation for the clients that you want to allow, and then choose **Save**\.

## Editing gateway level access settings for your SMB file share<a name="edit-smb-access-settings"></a>

You can set the security level for your gateway, set access for AD user, provide guests access, and set file share visibility for your file share\.

**Topics**
+ [Setting a security level for your gateway](#security-strategy)
+ [Using Active Directory to authenticate users](#enable-ad-settings)
+ [Providing guest access to your file share](#guest-access)
+ [Setting file share visibility](#file-share-visibility)

**To edit gateway level access settings for your SMB file share**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose the gateway that you want to use to join the domain\.

1. For **Actions**, choose **Edit SMB settings** to open the **Edit SMB settings** dialog box, and then choose the action that you want to perform\.

### Setting a security level for your gateway<a name="security-strategy"></a>

By using a S3 File, you can specify a security level for your gateway\. By specifying this security level, you can set whether your gateway should require Server Message Block \(SMB\) signing or SMB encryption, or whether you want to enable SMB version 1\.

**To configure security level**

1. Choose the pencil icon in the upper\-right corner of the **SMB security settings** section\.

1. For **Security level**, choose one of the following:
**Note**  
This setting is called `SMBSecurityStrategy` in the API Reference\.  
A higher security level can affect performance\.
   + **Enforce encryption** – If you choose this option, S3 File only allows connections from SMBv3 clients that have encryption enabled\. This option is highly recommended for environments that handle sensitive data\. This option works with SMB clients on Microsoft Windows 8, Windows Server 2012, or later\. 
   + **Enforce signing** – If you choose this option, S3 File only allows connections from SMBv2 or SMBv3 clients that have signing enabled\. This option works with SMB clients on Microsoft Windows Vista, Windows Server 2008, or later\. 
   + **Client negotiated** – If you choose this option, requests are established based on what is negotiated by the client\. This option is recommended when you want to maximize compatibility across different clients in your environment\.
**Note**  
For gateways activated before June 20, 2019, the default security level is **Client negotiated**\.  
For gateways activated on June 20, 2019 and later, the default security level is **Enforce encryption**\.

1. Choose **Save**\.

### Using Active Directory to authenticate users<a name="enable-ad-settings"></a>

To use your corporate Active Directory for user authenticated access to your SMB file share, edit the SMB settings for your gateway with your Microsoft AD domain credentials\. Doing this allows your gateway to join your Active Directory domain and allows members of the domain to access the SMB file share\.

**Note**  
Using AWS Directory Service, you can create a hosted Active Directory domain service in the AWS Cloud\.

Anyone who can provide the correct password gets guest access to the SMB file share\.

You can also enable access control lists \(ACLs\) on your SMB file share\. For information about how to enable ACLs, see \.

**To enable Active Directory authentication**

1. Choose the pencil icon in the upper\-right corner of the **Active Directory settings** section\.

1. For **Domain name**, provide the domain that you want the gateway to join\. You can join a domain by using its IP address or its organizational unit\. An *organizational unit* is an Active Directory subdivision that can hold users, groups, computers, and other organizational units\.
**Note**  
If your gateway can't join an Active Directory directory, try joining with the directory's IP address by using the [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html) API operation\.
**Note**  
**Active Directory status** shows **Detached** when a gateway has never joined a domain\.

1. Provide the domain user and the domain password, and then choose **Save**\.

   A message at the top of the **Gateways** section of your console indicates that your gateway successfully joined your AD domain\.

**To limit file share access to specific AD users and groups**

1. In the Storage Gateway console, choose the file share that you want to limit access to\.

1. For **Actions**, choose **Edit share settings** to open the **Edit Allowed/Denied users and groups** dialog box\.

1. For **Allowed users**, choose **Add entry** and provide the list of AD users that you want to allow file share access\.

1. For **Allowed groups**, choose **Add entry** and provide the list of AD groups that you want to allow file share access\.

1. For **Denied users**, choose **Add entry** and provide the list of AD users that you want to deny file share access\.

1. For **Denied groups**, choose **Add entry** and provide the list of AD users that you want to deny file share access\.

1. When you finish adding your entries, choose **Save**\.
**Note**  
For users and groups, enter only the AD user or group name\. The domain name is implied by the membership of the gateway in the specific AD that the gateway is joined to\.

If you don't specify valid or invalid users or groups, any authenticated Active Directory user can export the file share\.

### Providing guest access to your file share<a name="guest-access"></a>

If you want to provide only guest access, your S3 File doesn't have to be part of a Microsoft AD domain\. You can also use a S3 File that is a member of an AD domain to create file shares with guest access\. Before you create a file share using guest access, you need to change the default password\.

**To change the guest access password**

1. Choose the pencil icon in the upper\-right corner of the **Guest access settings** section\.

1. For **Guest password**, provide a password, and then choose **Save**\.

### Setting file share visibility<a name="file-share-visibility"></a>

File share visibility controls whether the shares on a gateway are visible when listing shares to users\. 

**To set file share visibility**

1. In the **File share visibility settings** section, choose the pencil icon to edit\.

1. For **Visibility status**, select the check box to have the shares on this gateway appear when listing shares to users\. Keep the check box cleared to have the shares on this gateway not appear when listing shares to users\.

## Editing settings for your SMB file share<a name="edit-smbfileshare-settings"></a>

After you have created an SMB file share, you can edit the storage class for your Amazon S3 bucket, object metadata, case sensitivity, access based enumeration, audit logs, automated cache refresh, and the export as settings for your file share\.

**Note**  
You cannot edit an existing file share to point to a new bucket or access point, or to modify the VPC endpoint settings\. You can configure those settings only when creating a new file share\.

**To edit SMB file share settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share that you want to update\.

1. For **Actions**, choose **Edit share settings**\.

1. Do one or more of the following:
   + \(Optional\) For **File share name**, enter a new name for the file share\.
   + For **Audit logs**, choose one of the following:
     + Choose **Disable logging** to turn off logging\.
     + Choose **Create a new log group** to create a new audit log\.
     + Choose **Use an existing log group**, and then choose an existing audit log from the list\.

     For more information about audit logs, see \.
   + \(Optional\) For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.
   + \(Optional\) For **File upload notification**, choose the check box to be notified when a file has been fully uploaded to S3 by the S3 File\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see \.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.
   + For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
     + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
     + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + For **Object metadata**, choose the metadata that you want to use:
     + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
     + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file's Network File System \(NFS\) or Server Message Block \(SMB\) file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](#cross-account-access)\.
     + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.
   + For **Export as**, choose an option for your file share\. The default value is **Read\-write**\.
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you select **Read\-only** for **Export as**, you might see an error message about an unexpected error keeping you from creating the folder\. This error message is a known issue with NFS version 3\. You can ignore the message\.
   + For **File/directory access controlled by**, choose one of the following:
     + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to control access to an SMB file share](smb-acl.md)\.
     + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

     If your authentication method is **Active Directory**, for **Admin users/groups**, enter a comma\-separated list of AD users and groups\. Do this if you want the admin user to have privileges to update ACLs on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the `@` character, for example, `@group1`\.
   + For **Case sensitivity**, select the check box to allow the gateway to control the case sensitivity, or clear the check box to allow the client to control the case sensitivity\.
**Note**  
If you are selecting this check box, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.
If you are clearing this check box, this setting might cause you to loss access to files with names that differ only in their case\.
   + For **Access based enumeration**, select the check box to make the files and folders on the share visible only to users who have read access\. Keep the check box cleared to make the files and folders on the share visible to all users during directory enumeration\.
**Note**  
Access\-based enumeration is a system that filters the enumeration of files and folders on an SMB file share based on the share's access control lists \(ACLs\)\.
   + For **Opportunistic lock \(oplock\)**, choose one of the following:
     + Choose **Enabled** to allow the file share to use opportunistic locking to optimize the file buffering strategy, which improves performance in most cases, particularly with regard to Windows context menus\.
     + Choose **Disabled** to prevent the use of opportunistic locking\. If multiple Windows clients in your environment frequently edit the same files simultaneously, disabling opportunistic locking can sometimes improve performance\.
**Note**  
Enabling opportunistic locking on case\-sensitive shares is not recommended for workloads that involve access to files with the same name in different case\.

1. Choose **Save**\.

## Refreshing objects in your Amazon S3 bucket<a name="refresh-cache"></a>

As your NFS or SMB client performs file system operations, your gateway maintains an inventory of the objects in the S3 bucket associated with your file share\. Your gateway uses this cached inventory to reduce the latency and frequency of S3 requests\. This operation does not import files into the S3 File cache storage\. It only updates the cached inventory to reflect changes in the inventory of the objects in the S3 bucket\.

To refresh the S3 bucket for your file share, you can use the Storage Gateway console, the [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) operation in the Storage Gateway API, or an AWS Lambda function\.

**To refresh objects in an S3 bucket from the console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **File shares**, and then choose the file share associated with the S3 bucket that you want to refresh\.

1. For **Actions**, choose **Refresh cache**\.

   The time that the refresh process takes depends on the number of objects cached on the gateway and the number of objects that were added to or removed from the S3 bucket\.

**To refresh objects in an S3 bucket using an AWS Lambda function**

1. Identify the S3 bucket used by the S3 File\.

1. Check that the *Event* section is blank\. It populates automatically later\.

1. Create an IAM role, and allow Trust Relationship for Lambda `lambda.amazonaws.com`\.

1. Use the following policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "StorageGatewayPermissions",
               "Effect": "Allow",
               "Action": "storagegateway:RefreshCache",
               "Resource": "*"
           },
           {
               "Sid": "CloudWatchLogsPermissions",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Create a Lambda function from the Lambda console\.

1. Use the following function for your Lambda task\.

   ```
   import json
   import boto3
   client = boto3.client('storagegateway')
   def lambda_handler(event, context):
       print(event)
       response = client.refresh_cache(
           FileShareARN='arn:aws:storagegateway:ap-southeast-2:672406774878:share/share-E51FBD9C'
       )
       print(response)
       return 'Your FileShare cache has been refreshed'
   ```

1. For **Execution role**, choose the IAM role you created\.

1. Optional: add a trigger for Amazon S3 and select the event **ObjectCreated** or **ObjectRemoved**\.
**Note**  
`RefreshCache` needs to complete one process before starting another\. When you create or delete many objects in a bucket, performance might degrade\. Therefore, we recommend against using S3 triggers\. Instead, use the Amazon CloudWatch rule described following\.

1. Create a CloudWatch rule on the CloudWatch console and add a schedule\. Generally, we recommend a *fixed rate* of 30 minutes\. However, you can use 1\-2 hours on large S3 bucket\.

1. Add a new trigger for CloudWatch events and choose the rule you just created\.

1. Save your Lambda configuration\. Choose **Test**\.

1. Choose **S3 PUT** and customize the test to your requirements\.

1. The test should succeed\. If not, modify the JSON to your requirements and retest\.

1. Open the Amazon S3 console, and verify that the event your created and the Lambda function ARN are present\.

1. Upload an object to your S3 bucket using the Amazon S3 console or the AWS CLI\.

   The CloudWatch console generates a CloudWatch output similar to the following\.

   ```
   {
       u'Records': [
           {u'eventVersion': u'2.0', u'eventTime': u'2018-09-10T01:03:59.217Z', u'requestParameters': {u'sourceIPAddress': u'MY-IP-ADDRESS'}, 
           u's3': {u'configurationId': u'95a51e1c-999f-485a-b994-9f830f84769f', u'object': {u'sequencer': u'00549CC2BF34D47AED', u'key': u'new/filename.jpeg'}, 
           u'bucket': {u'arn': u'arn:aws:s3:::MY-BUCKET', u'name': u'MY-GATEWAY-NAME', u'ownerIdentity': {u'principalId': u'A3OKNBZ72HVPP9'}}, u's3SchemaVersion': u'1.0'}, 
           u'responseElements': {u'x-amz-id-2': u'76tiugjhvjfyriugiug87t890nefevbck0iA3rPU9I/s4NY9uXwtRL75tCyxasgsdgfsq+IhvAg5M=', u'x-amz-request-id': u'651C2D4101D31593'}, 
           u'awsRegion': u'MY-REGION', u'eventName': u'ObjectCreated:PUT', u'userIdentity': {u'principalId': u'AWS:AROAI5LQR5JHFHDFHDFHJ:MY-USERNAME'}, u'eventSource': u'aws:s3'}
       ]
   }
   ```

   The Lambda invocation gives you output similar to the following\.

   ```
   {
       u'FileShareARN': u'arn:aws:storagegateway:REGION:ACCOUNT-ID:share/MY-SHARE-ID', 
           'ResponseMetadata': {'RetryAttempts': 0, 'HTTPStatusCode': 200, 'RequestId': '6663236a-b495-11e8-946a-bf44f413b71f', 
               'HTTPHeaders': {'x-amzn-requestid': '6663236a-b495-11e8-946a-bf44f413b71f', 'date': 'Mon, 10 Sep 2018 01:03:59 GMT', 
                   'content-length': '90', 'content-type': 'application/x-amz-json-1.1'
           }
       }
   }
   ```

   Your NFS share mounted on your client will reflect this update\.
**Note**  
For caches updating large object creation or deletion in large buckets with millions of objects, updates may take hours\.

1. Delete your object manually using the Amazon S3 console or AWS CLI\.

1. View the NFS share mounted on your client\. Verify that your object is gone \(because your cache refreshed\)\.

1. Check your CloudWatch logs to see the log of your deletion with the event `ObjectRemoved:Delete`\.

   ```
   {
       u'account': u'MY-ACCOUNT-ID', u'region': u'MY-REGION', u'detail': {}, u'detail-type': u'Scheduled Event', u'source': u'aws.events', 
       u'version': u'0', u'time': u'2018-09-10T03:42:06Z', u'id':  u'6468ef77-4db8-0200-82f0-04e16a8c2bdb', 
       u'resources': [u'arn:aws:events:REGION:MY-ACCOUNT-ID:rule/FGw-RefreshCache-CW']
   }
   ```
**Note**  
For cron jobs or scheduled tasks, your CloudWatch log event is `u'detail-type': u'Scheduled Event'`\.

Refreshing the cache only initiates the refresh operation\. When the cache refresh completes, it doesn't necessarily mean that the file refresh is complete\. To determine that the file refresh operation is complete before you check for new files on the gateway file share, use the `refresh-complete` notification\. To do this, you can subscribe to be notified through an Amazon CloudWatch event when your [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html) operation completes\. For more information, see \.

## Using S3 object lock with a Amazon S3 File Gateway<a name="s3-object-lock"></a>

Amazon S3 File Gateway supports accessing S3 buckets that have Amazon S3 Object Lock enabled\. Amazon S3 Object Lock enables you to store objects using a "Write Once Read Many" \(WORM\) model\. When you use Amazon S3 Object Lock, you can prevent an object in your S3 bucket from being deleted or overwritten\. Amazon S3 Object Lock works together with object versioning to protect your data\.

If you enable Amazon S3 Object Lock, you can still modify the object\. For example, it can be written to, deleted, or renamed through a file share on a S3 File\. When you modify an object in this way, S3 File places a new version of the object without affecting the previous version \(that is, the locked object\)\. 

For example, If you use the S3 File NFS or SMB interface to delete a file and the corresponding S3 object is locked, the gateway places an S3 delete marker as the next version of the object, and leaves the original object version in place\. Similarly, If a S3 File modifies the contents or metadata of a locked object, a new version of the object is uploaded with the changes, but the original locked version of the object remains unchanged\. 

For more information about Amazon S3 Object Lock, see [Locking objects using S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lock.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Understanding file share status<a name="understand-file-share"></a>

Each file share has an associated status that tells you at a glance what the health of the file share is\. Most of the time, the status indicates that the file share is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem that might or might not require action on your part\.

You can see file share status on the Storage Gateway console\. File share status appears in the **Status** column for each file share in your gateway\. A file share that is functioning normally has the status of AVAILABLE\.

In the following table, you can find a description of each file share status, and if and when you should act based on the status\. A file share should have AVAILABLE status all or most of the time it's in use\.


| Status | Meaning | 
| --- | --- | 
| AVAILABLE |  The file share is configured properly and is available to use\. The AVAILABLE status is the normal running status for a file share\.  | 
| CREATING |  The file share is being created and is not ready for use\. The CREATING status is transitional\. No action is required\. If file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| UPDATING |  The file share configuration is being updated\. If a file share is stuck in this status, it's probably because the gateway VM lost connection to AWS\.  | 
| DELETING |  The file share is being deleted\. The file share is not deleted until all data is uploaded to AWS\. The DELETING status is transitional, and no action is required\.  | 
| FORCE\_DELETING |  The file share is being deleted forcibly\. The file share is deleted immediately and uploading to AWS is aborted\. The FORCE\_DELETING status is transitional, and no action is required\.  | 
| UNAVAILABLE |  The file share is in an unhealthy state\. Certain issues can cause the file share to go into an unhealthy state\. For example, role policy errors can cause this, or if the file share maps to an Amazon S3 bucket that doesn't exist\. When the issue that caused the unhealthy state is resolved, the file returns to AVAILABLE state\.  | 

## File share best practices<a name="fileshare-best-practices"></a>

In this section, you can find information about best practices for creating file shares\.

**Topics**
+ [Preventing multiple file shares writing to your Amazon S3 bucket](#prevent-multiple-writes)
+ [Allowing specific NFS clients to mount your file share](#nfs-client-mount)

### Preventing multiple file shares writing to your Amazon S3 bucket<a name="prevent-multiple-writes"></a>

When you create a file share, we recommend that you configure your Amazon S3 bucket so that only one file share can write to it\. If you configure your S3 bucket to be written to by multiple file shares, unpredictable results can occur\. To prevent this, create an S3 bucket policy that denies all roles except the role used for the file share to put or delete objects in the bucket\. Then attach this policy to the S3 bucket\.

The following example policy denies all roles except the role that created the bucket to write to the S3 bucket\. The `s3:DeleteObject` and `s3:PutObject` actions are denied for all roles except `"TestUser"`\. The policy applies to all objects in the `"arn:aws:s3:::TestBucket/*"` bucket\.

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

### Allowing specific NFS clients to mount your file share<a name="nfs-client-mount"></a>

We recommend that you change the allowed NFS client settings for your file share\. If you don't, any client on your network can mount your file share\. For information about how to edit your NFS client settings, see [Editing access settings for your NFS file share](#edit-nfs-client)\.