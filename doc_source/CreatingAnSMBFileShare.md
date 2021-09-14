# Creating an SMB file share<a name="CreatingAnSMBFileShare"></a>

Before you create an SMB file share, make sure that you configure SMB security settings for your file gateway\. You also configure either Microsoft Active Directory \(AD\) or guest access for authentication\. A file share provides one type of SMB access only\.

**Note**  
An SMB file share doesn't operate correctly without the needed ports open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

## Configuring SMB settings<a name="configure-SMB-settings"></a>

**To configure SMB settings for your file gateway:**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Gateways**, and on the **Gateway** page, select the check box next to the file gateway that you want to join to a domain\.

1. For **Actions**, choose **Edit SMB settings**\.

At this point, you can configure settings for your file gateway:
+ [Configuring SMB security settings](#configure-SMB-security-settings)
+ [Configuring Microsoft Active Directory access](#configure-SMB-file-share-AD-access)
+ [Configuring guest access](#configure-SMB-file-share-guest-access)
+ [Creating an SMB file share](#create-SMB-file-share)

## Configuring SMB security settings<a name="configure-SMB-security-settings"></a>

Use the following procedure to configure your file gateway SMB security settings\.

**To configure SMB security settings**

1. Choose the pencil icon in the upper right corner of the **SMB security settings** section\.

1. For **Security level**, choose one of the following:
**Note**  
This setting is called `SMBSecurityStrategy` in the API Reference\.  
A higher security level can affect performance\.
   + **Enforce encryption** – if you choose this option, file gateway only allows connections from SMBv3 clients that have encryption enabled\. This option is highly recommended for environments that handle sensitive data\. This option works with SMB clients on Microsoft Windows 8, Windows Server 2012, or newer\.
   + **Enforce signing** – if you choose this option, file gateway only allows connections from SMBv2 or SMBv3 clients that have signing enabled\. This option works with SMB clients on Microsoft Windows Vista, Windows Server 2008, or newer\.
   + **Client negotiated** – if you choose this option, requests are established based on what is negotiated by the client\. This option is recommended when you want to maximize compatibility across different clients in your environment\.
**Note**  
For gateways activated before June 20, 2019, the default security level is **Client negotiated**\.  
For gateways activated on June 20, 2019 and later, the default security level is **Enforce encryption**\.

1. Choose **Close** if you are done\.

## Configuring Microsoft Active Directory access<a name="configure-SMB-file-share-AD-access"></a>

Use the following procedure to configure your file gateway Microsoft AD access settings\.

**To configure your SMB file share Microsoft AD access settings**

1. Choose the pencil icon in the upper right corner of the **Active Directory settings** section\.

1. For **Domain name**, provide the domain that you want the gateway to join\. You can join a domain by using its IP address or its organizational unit\. An *organizational unit* is an Active Directory subdivision that can hold users, groups, computers, and other organizational units\.
**Note**  
You can use the [AWS Directory Service](http://aws.amazon.com/directoryservice/) to create a hosted Microsoft AD domain service in the AWS Cloud\.  
If your gateway can't join a Microsoft AD directory, try joining with the directory's IP address by using the [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html) API operation\.  
**Active Directory status** shows **Detached** when a gateway has never joined a domain\.

1. For **Domain user**, enter your account name\. Your account must be able to join a server to a domain\.

1. For **Domain password**, enter your account password\.

1. \(Optional\) For **Organizational unit**, enter your organizational unit\.

1. \(Optional\) For **Domain controller\(s\)**, enter a comma\-separated list of Internet Protocol version 4 \(IPv4\) addresses, NetBIOS names, or hostnames of your domain server\.

1. Choose **Save** to save your changes\.

1. Choose **Close** if you are done\.

## Configuring guest access<a name="configure-SMB-file-share-guest-access"></a>

Use the following procedure to configure your file gateway guess access settings\.

**To configure your SMB file share for guest access**

1. Choose the pencil icon in the upper right corner of the **Guest access settings** section\.

1. For **Guest password**, enter a password that meets your organization's security requirements\.

1. Choose **Save** to complete the authentication\.
**Note**  
If you provide only guest access, your file gateway doesn't have to be part of an AD domain\. You can also use a file gateway that is a member of your Microsoft AD domain to create file shares with guest access\.

1. Choose **Close** if you are done\.

A message at the top of the **Gateways** section of your console should appear, saying that your gateway `Successfully joined domain`\.

If the banner displays the message `Invalid domain name/DNS name cannot be resolved`, the correct endpoint wasn't found\. You might also see the error `Invalid users/Invalid password`, an authentication failure that means that your logon was not recognized by the domain service\.

The error message `The gateway cannot connect to the specified domain` can indicate that the quota of users has been exhausted, in other words there are no more users in the quota\. The default limit allows each user to join up to 10 systems to a domain\. This error can also appear if the user that tried to connect didn't have administrator privileges\.

The error message `The specified request timed out` might indicate that there is a problem with your firewall rules not allowing access to the domain\.

## Creating an SMB file share<a name="create-SMB-file-share"></a>

In this procedure, you create an SMB file share with either Microsoft AD or guest access\. Make sure that you define the SMB file share settings for your file gateway before performing the following steps\. To configure the SMB file share settings, see [Configuring SMB settings](#configure-SMB-settings)\.

**To create an SMB file share**

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

1. For **Access objects using**, choose **Server Message Block \(SMB\)**\.

1. For **Gateway**, make sure that your gateway is chosen\.

1. For **Audit logs**, choose one of the following:
   + **Disable logging**
   + **Create a new log group** to create a new audit log\.
   + **Use an existing log group** and choose an existing audit log from the list\.

   For more information about audit logs, see [Understanding File Gateway Audit Logs](monitoring-file-gateway.md#audit-logs)\.

1. \(Optional\) For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh after which access to the directory would cause the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file SMB file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your file gateway to use to access your Amazon S3 bucket:
   + **Create a new IAM role** to enable the file gateway to create a new IAM role and access the policy on your behalf\.
   + **Use an existing IAM role** to choose an existing IAM role and to set up the access policy manually\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your file gateway stores in Amazon S3:
   + Choose **S3\-Managed Keys \(SSE\-S3\)** to use server\-side encryption managed with Amazon S3 \(SSE\-S3\)\.
   + Choose **KMS\-Managed Keys \(SSE\-KMS\)** to use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\)\. In the **Master key** box, choose an existing master KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric customer master keys \(CMKs\) are not supported\.

1. Choose **Next** to review configuration settings for your file share\. Your file gateway applies default settings to your file share\.

**To change the configuration settings for your SMB file share**

1. Choose **Edit** for the settings that you want to change\. Choose **Close** to enforce your settings\.

1. For **SMB share settings**, you can edit the authentication method, export as, file and directory access, and admin user and groups\.

   For **Select authentication method**, choose one of the following:
   + **Active Directory** \(the default value\) to use your corporate Microsoft AD for user authenticated access to your SMB file share\.
   + **Guest access** to provide only guest access; your file gateway doesn't have to be part of a Microsoft AD domain\. You can also use a file gateway that is a member of an AD domain to create file shares with guest access\.
**Note**  
For Microsoft AD access, your file gateway must be joined to a domain\.  
For guest access, you must set a guest access password\.  
Both access types are available at the same time\.

   For **Export as**, choose one of the following:
   + **Read\-write** \(the default value\)
   + **Read\-only**
**Note**  
For file shares mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

   For **File/directory access controlled by**, choose one of the following:
   + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.
   + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

   \(Optional\) For **Admin users/groups**, enter a comma\-separated list of AD users and groups\. You do this if you want the admin user to have privileges to update ACLs on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the @ character, for example `@group1`\.

   For **Case sensitivity**, select the check box to allow the gateway to control the case sensitivity or leave the check box blank to allow the client to control the case sensitivity\.
**Note**  
If selecting, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.

1. \(Optional\) For **Allowed/denied users and groups**, choose **Add entry** and provide the list of AD users or groups that you want to allow or deny file share access\.
**Note**  
The **Allowed/denied users and groups** section appears only if **Active Directory** is selected\.  
Enter only the AD user or group name\. The domain name is implied by the membership of the gateway in the specific AD that the gateway is joined to\.  
If you don't specify valid or invalid users or groups, any authenticated AD user can export the file share\.

1. For **Tags**, you add new tags or remove existing tags\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your SMB file share is created, you can see your file share settings in the file share's **Details** tab\.

The preceding procedure creates a Microsoft AD file share\. Anyone with domain credentials can access this file share\. To limit access to certain users and groups, see [Using Active Directory to authenticate users](managing-gateway-file.md#enable-ad-settings)\.

**Next Step**

[Mounting your SMB file share on your client](using-smb-fileshare.md)