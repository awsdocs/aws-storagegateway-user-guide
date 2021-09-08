# Create an SMB file share<a name="CreatingAnSMBFileShare"></a>

Before you create an SMB file share, make sure that you configure SMB security settings for your file gateway\. You also must configure either Microsoft Active Directory \(AD\) or guest access for authentication\. A file share provides one type of SMB access only\.



**Note**  
An SMB file share doesn't operate correctly unless the required ports are open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

**Note**  
When a file is written to the file gateway by an SMB client, the file gateway uploads the file's data to Amazon S3 followed by its metadata \(ownerships, timestamps, etc\.\)\. Uploading the file data creates an S3 object, and uploading the metadata for the file updates the metadata for the S3 object\. This process creates another version of the object, resulting in two versions of an object\. If S3 Versioning is enabled, both versions will be stored\.  
If you change the metadata of a file stored in your file gateway, a new S3 object is created and replaces the existing S3 object\. This behavior is different from editing a file in a file system, where editing a file does not result in a new file being created\. You should test all file operations that you plan to use with Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
The use of S3 Versioning and Cross\-Region replication \(CRR\) in Amazon S3 should be carefully considered when data is being uploaded from your file gateway\. Uploading files from your file gateway to Amazon S3 when S3 Versioning is enabled results in at least two versions of a S3 object\.  
Certain workflows involving large files and file\-writing patterns such as file uploads that are performed in several steps can increase the number of stored S3 object versions\. If the file gateway cache needs to free up space due to high file\-write rates, multiple S3 object versions might be created\. These scenarios increase S3 storage if S3 Versioning is enabled and increase transfer costs associated with CRR\. You should test all file operations that you plan to use with Storage Gateway so that you understand how each file operation interacts with Amazon S3 storage\.  
Using the Rsync utility with your file gateway results in the creation of temporary files in the cache and the creation of temporary S3 objects in Amazon S3\. This situation results in early deletion charges in the S3 Standard\-Infrequent Access \(S3 Standard\-IA\) and S3 Intelligent\-Tiering storage classes\.

## Create SMB settings<a name="configure-SMB-settings"></a>

------
#### [ New Console ]

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share** to open the **File share settings** page\.

1. For **Gateway**, choose your Amazon S3 File Gateway from the list\.

1. For **Amazon S3 location**, do one of the following:
   + To connect the file share directly to an S3 bucket, choose **S3 bucket name**, then enter the bucket name and, optionally, a prefix name for objects created by the file share\. Your gateway uses this bucket to store and retrieve files\. For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon Simple Storage Service Console User Guide*\.
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

1. For **Access objects using**, choose **Server Message Block \(SMB\)**\.

1. For **Audit logs**, choose one of the following:
   + Choose **Disable logging** to turn off logging\.
   + Choose **Create a new log group** to create a new audit log\.
   + Choose **Use an existing log group**, and then choose an existing audit log from the list\.

   For more information about audit logs, see [Understanding file gateway audit logs](monitoring-file-gateway.md#audit-logs)\.

1. For **Automated cache refresh from S3**, choose **Set refresh interval**, and then set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. For **File upload notification**, choose **Settling time \(seconds\)** to be notified when a file has been fully uploaded to S3 by the file gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. \(Optional\) In the **Tags** section, choose **Add new tag**, and then enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Amazon S3 storage settings** page appears\.

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

1. Choose **Next**\. The **File access settings** page appears\.

1. For **Authentication method**, choose the authentication method that you want to use\.
   + Choose **Active Directory** to use your corporate Microsoft AD for user authenticated access to your SMB file share\. Your file gateway must be joined to a domain\.
   + Choose **Guest access** to provide only guest access; your file gateway doesn't have to be part of a Microsoft AD domain\. You can also use a file gateway that is a member of an AD domain to create file shares with guest access\. You must set a guest password for your SMB server in the corresponding field\.
**Note**  
Both access types are available at the same time\.

1. In the **SMB share settings** section, choose your settings\.

   For **Export as**, choose one of the following:
   + **Read\-write** \(the default value\)
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error preventing you from creating the folder\. You can ignore this message\.

   For **File/directory access controlled by**, choose one of the following:
   + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to control access to an SMB file share](smb-acl.md)\.
   + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

   If your authentication method is **Active Directory**, for **Admin users/groups**, enter a comma\-separated list of AD users and groups\. Do this if you want the admin user to have privileges to update access control lists \(ACLs\) on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the `@` character, for example, `@group1`\.

    For **Case sensitivity**, choose **Client specified** to allow the gateway to control the case sensitivity, or choose **Force case sensitivity** to allow the client to control the case sensitivity\.
**Note**  
If selected, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.

   For **Access based enumeration**, choose **Disabled** for files and directories to make the files and folders on the share visible only to users who have read access, or choose **Enabled for files and directories** to make the files and folders on the share visible to all users during directory enumeration\.
**Note**  
Access\-based enumeration is a system that filters the enumeration of files and folders on an SMB file share based on the share's access control lists \(ACLs\)\.

   For **Opportunistic lock \(oplock\)**, choose one of the following:
   + Choose **Enabled** to allow the file share to use opportunistic locking to optimize the file buffering strategy, which improves performance in most cases, particularly with regard to Windows context menus\.
   + Choose **Disabled** to prevent the use of opportunistic locking\. If multiple Windows clients in your environment frequently edit the same files simultaneously, disabling opportunistic locking can sometimes improve performance\.
**Note**  
Enabling opportunistic locking on case\-sensitive shares is not recommended for workloads that involve access to files with the same name in different case\.

1. \(Optional\) In the **User and group file share access** section, choose your settings\.

   For **Allowed users and groups**, choose **Add allowed user** or **Add allowed group** and enter an AD user or group that you want to allow file share access\. Repeat this process to allow as many users and groups as necessary\.

   For **Denied users and groups**, choose **Add denied user** or **Add denied group** and enter an AD user or group that you want to deny file share access\. Repeat this process to deny as many users and groups as necessary\.
**Note**  
The **User and group file share access** section appears only if **Active Directory** is selected\.  
Enter only the AD user or group name\. The domain name is implied by the membership of the gateway in the specific AD that the gateway is joined to\.  
If you don't specify valid or invalid users or groups, any authenticated AD user can export the file share\.

1. Choose **Next**\.

1. Review your file share configuration settings, and then choose **Finish**\.

   After your SMB file share is created, you can see your file share settings in the file share's **Details** tab\.

------
#### [ Original Console ]

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/)\.

1. Choose **Create file share**\.

1. On the **Configure file share settings** page, for **Amazon S3 bucket name / Prefix name**, do one or more of the following:
   + In *Existing S3 bucket name*, enter the name for an existing Amazon S3 bucket\. You use this bucket for your gateway to store files in and retrieve\. For information about creating a new bucket, see [How do I create an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.
   + \(Optional\) In *S3 prefix name*, enter a prefix name for the S3 bucket\. For information about using prefix names, see [ Organizing objects using prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html) in the *Amazon Simple Storage Service Console User Guide*\.
   + 
**Note**  
The prefix name must end with a forward slash \(`/)`\.
If you enter a prefix name, you must enter a file share name\.
After the file share is created, the prefix name can't be modified or deleted\.

1. \(Optional\) For **File share name**, enter a name for the file share\. The default name is the S3 bucket name\.
**Note**  
If you enter a prefix name, you must enter a file share name\.
After the file share is created, the file share name can't be deleted\.

1. For **Access objects using**, choose **Server Message Block \(SMB\)**\.

1. For **Gateway**, make sure that your gateway is chosen\.

1. For **Audit logs**, choose one of the following:
   + Choose **Disable logging** to turn off logging\.
   + Choose **Create a new log group** to create a new audit log\.
   + Choose **Use an existing log group**, and then choose an existing audit log from the list\.

   For more information about audit logs, see [Understanding file gateway audit logs](monitoring-file-gateway.md#audit-logs)\.

1. \(Optional\) For **Automated cache refresh from S3 after**, select the check box and set the time in days, hours, and minutes to refresh the file share's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. After the TTL interval has elapsed, accessing the directory causes the file gateway to first refresh that directory's contents from the Amazon S3 bucket\.

1. \(Optional\) For **File upload notification**, choose the check box to be notified when a file has been fully uploaded to S3 by the file gateway\. Set the **Settling Time** in seconds to control the number of seconds to wait after the last point in time that a client wrote to a file before generating an `ObjectUploaded` notification\. Because clients can make many small writes to files, it's best to set this parameter for as long as possible to avoid generating multiple notifications for the same file in a small time period\. For more information, see [Getting file upload notification](monitoring-file-gateway.md#get-file-upload-notification)\.
**Note**  
This setting has no effect on the timing of the object uploading to S3, only on the timing of the notification\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\.

1. Choose **Next**\. The **Configure how files are stored in Amazon S3** page appears\.

**Configure how files are stored in Amazon S3**

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard storage class, see [Storage classes for frequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-freq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Intelligent\-Tiering** to optimize storage costs by automatically moving data to the most cost\-effective storage access tier\. For more information about the S3 Intelligent\-Tiering storage class, see [Storage class for automatically optimizing frequently and infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-dynamic-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\. For more information about the S3 Standard\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\. For more information about the S3 One Zone\-IA storage class, see [Storage classes for infrequently accessed objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata that you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the SMB file share\. For more information about using your file share to access objects in a bucket owned by another account, see [Using a file share for cross\-account access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of the bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your S3 bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your file gateway to use to access your Amazon S3 bucket:
   + Choose **Create a new IAM role** to enable the file gateway to create a new IAM role and access the policy on your behalf\.
   + Choose **Use an existing IAM role** to choose an existing IAM role and to set up the access policy manually\. In the **IAM role** box, enter the Amazon Resource Name \(ARN\) for the role used to access your bucket\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\.

   For more information about access to your S3 bucket, see [Granting access to an Amazon S3 bucket](managing-gateway-file.md#grant-access-s3)\.

1. For **Encryption**, choose the type of encryption keys to use to encrypt objects that your file gateway stores in Amazon S3:
   + Choose **S3\-Managed Keys \(SSE\-S3\)** to use server\-side encryption managed with Amazon S3 \(SSE\-S3\)\.
   + Choose **KMS\-Managed Keys \(SSE\-KMS\)** to use server\-side encryption managed with AWS Key Management Service \(SSE\-KMS\)\. In the **Master key** box, choose an existing master KMS key or choose **Create a new KMS key** to create a new KMS key in the AWS Key Management Service \(AWS KMS\) console\. For more information about AWS KMS, see [What is AWS Key Management Service? ](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
To specify an AWS KMS key with an alias that is not listed or to use an AWS KMS key from a different AWS account, you must use the AWS Command Line Interface \(AWS CLI\)\. For more information, see [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html) in the *AWS Storage Gateway API Reference*\.  
Asymmetric customer master keys \(CMKs\) are not supported\.

1. Choose **Next** to review the configuration settings for your file share\. Your file gateway applies default settings to your file share\. You can choose the **Edit** button and change the settings for **Allowed clients**,** Mount options**, and **File metadata defaults**\.

At this point, you can configure the settings for your file gateway\.

**To change the configuration settings for your SMB file share**

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

   After your SMB file share is created, you can see your file share settings in the file share's **Details** tab\.

------
+ [Configuring SMB security settings](#configure-SMB-security-settings)
+ [Configuring Microsoft Active Directory access](#configure-SMB-file-share-AD-access)
+ [Configuring guest access](#configure-SMB-file-share-guest-access)
+ [Creating an SMB file share with Active Directory or guest access](#create-SMB-file-share)

## Configuring SMB security settings<a name="configure-SMB-security-settings"></a>

Use the following procedure to configure your file gateway SMB security settings\.

------
#### [ Original Console Only ]

**To configure SMB security settings**

1. Choose the pencil icon in the upper\-right corner of the **SMB security settings** section\.

1. For **Security level**, choose one of the following:
**Note**  
This setting is called `SMBSecurityStrategy` in the API Reference\.  
A higher security level can affect performance\.
   + **Enforce encryption** – If you choose this option, file gateway only allows connections from SMBv3 clients that have encryption enabled\. This option is highly recommended for environments that handle sensitive data\. This option works with SMB clients on Microsoft Windows 8, Windows Server 2012, or later\.
   + **Enforce signing** – If you choose this option, file gateway only allows connections from SMBv2 or SMBv3 clients that have signing enabled\. This option works with SMB clients on Microsoft Windows Vista, Windows Server 2008, or later\.
   + **Client negotiated** – If you choose this option, requests are established based on what is negotiated by the client\. This option is recommended when you want to maximize compatibility across different clients in your environment\.
**Note**  
For gateways activated before June 20, 2019, the default security level is **Client negotiated**\.  
For gateways activated on June 20, 2019 and later, the default security level is **Enforce encryption**\.

1. Choose **Close** if you are done\.

------

## Configuring Microsoft Active Directory access<a name="configure-SMB-file-share-AD-access"></a>

Use the following procedure to configure your file gateway Microsoft AD access settings\.

------
#### [ Original Console Only ]

**To configure your SMB file share Microsoft AD access settings**

1. Choose the pencil icon in the upper\-right corner of the **Active Directory settings** section\.

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

------

## Configuring guest access<a name="configure-SMB-file-share-guest-access"></a>

Use the following procedure to configure your file gateway guess access settings\.

------
#### [ Original Console only ]

**To configure your SMB file share for guest access**

1. Choose the pencil icon in the upper\-right corner of the **Guest access settings** section\.

1. For **Guest password**, enter a password that meets your organization's security requirements\.

1. Choose **Save** to complete the authentication\.
**Note**  
If you provide only guest access, your file gateway doesn't have to be part of an AD domain\. You can also use a file gateway that is a member of your Microsoft AD domain to create file shares with guest access\.

1. Choose **Close** if you are done\.

------

A message at the top of the **Gateways** section of your console should appear, saying that your gateway `Successfully joined domain`\.

If the banner displays the message `Invalid domain name/DNS name cannot be resolved`, the correct endpoint wasn't found\. You might also see the error `Invalid users/Invalid password`\. This authentication failure means that your logon was not recognized by the domain service\.

The error message `The gateway cannot connect to the specified domain` can indicate that the quota of users has been exhausted; in other words there are no more users in the quota\. The default limit allows each user to join up to 10 systems to a domain\. This error can also appear if the user that tried to connect didn't have administrator privileges\.

The error message `The specified request timed out` might indicate that there is a problem with your firewall rules not allowing access to the domain\.

## Creating an SMB file share with Active Directory or guest access<a name="create-SMB-file-share"></a>

In this procedure, you create an SMB file share with either Microsoft AD or guest access\. Make sure that you define the SMB file share settings for your file gateway before performing the following steps\. To configure the SMB file share settings, see [Create SMB settings](#configure-SMB-settings)\.

------
#### [ New Console ]

**To configure file access settings for your SMB file share**

1. For **Select authentication method**, choose the authentication method that you want to use\.
   + Choose **Active Directory** \(the default value\) to use your corporate Microsoft AD for user authenticated access to your SMB file share\.
   + Choose **Guest access** to provide only guest access; your file gateway doesn't have to be part of a Microsoft AD domain\. You can also use a file gateway that is a member of an AD domain to create file shares with guest access\.
**Note**  
For Microsoft AD access, your file gateway must be joined to a domain\.  
For guest access, you must set a guest access password\.  
Both access types are available at the same time\.

1. For **Guest password**, enter the password for your gateway\. The gateway must have a guest password set on it\.

1. In the **SMB share settings** section, choose your settings\.

   For **Export as**, choose one of the following:
   + **Read\-write** \(the default value\)
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

   For **File/directory access controlled by**, choose one of the following:
   + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to control access to an SMB file share](smb-acl.md)\.
   + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

   If your authentication method is **Active Directory**, for **Admin users/groups**, enter a comma\-separated list of AD users and groups\. Do this if you want the admin user to have privileges to update access control lists \(ACLs\) on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the `@` character, for example, `@group1`\.

   For **Case sensitivity**, choose **Client specified** to allow the gateway to control the case sensitivity, or choose **Force case sensitivity** to allow the client to control the case sensitivity\.
**Note**  
If selected, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.

   For **Access based enumeration**, choose **Disabled for files and directories** to make the files and folders on the share visible only to users who have read access, or choose **Enabled for files and directories** to make the files and folders on the share visible to all users during directory enumeration\.
**Note**  
Access\-based enumeration is a system that filters the enumeration of files and folders on an SMB file share based on the share's access control lists \(ACLs\)\.

1. Choose **Next**\.

1. Review your file share configuration settings, and then choose **Finish**\.

   After your SMB file share is created, you can see your file share settings in the file share's **Details** tab\.

------
#### [ Original Console ]

**To change the configuration settings for your SMB file share**

1. Choose **Edit** for the settings that you want to change\. Choose **Close** to enforce your settings\.

1. For **SMB share settings**, you can edit the authentication method, export as, file and directory access, and admin user and groups\.

   For **Select authentication method**, choose one of the following:
   + Choose **Active Directory** \(the default value\) to use your corporate Microsoft AD for user authenticated access to your SMB file share\.
   + Choose **Guest access** to provide only guest access; your file gateway doesn't have to be part of a Microsoft AD domain\. You can also use a file gateway that is a member of an AD domain to create file shares with guest access\.
**Note**  
For Microsoft AD access, your file gateway must be joined to a domain\.  
For guest access, you must set a guest access password\.  
Both access types are available at the same time\.

   For **Export as**, choose one of the following:
   + **Read\-write** \(the default value\)
   + **Read\-only**
**Note**  
For file shares that are mounted on a Microsoft Windows client, if you choose **Read\-only**, you might see a message about an unexpected error keeping you from creating the folder\. You can ignore this message\.

   For **File/directory access controlled by**, choose one of the following:
   + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to control access to an SMB file share](smb-acl.md)\.
   + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

   \(Optional\) For **Admin users/groups**, enter a comma\-separated list of AD users and groups\. Do this if you want the admin user to have privileges to update access control lists \(ACLs\) on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the `@` character, for example, `@group1`\.

   \(Optional\) For **Case sensitivity**, select the check box to allow the gateway to control the case sensitivity, or keep the check box cleared to allow the client to control the case sensitivity\.
**Note**  
If selected, this setting applies immediately to new SMB client connections\. Existing SMB client connections must disconnect from the file share and reconnect for the setting to take effect\.

   \(Optional\) For **Access based enumeration**, select the check box to make the files and folders on the share visible only to users who have read access\. Keep the check box cleared to make the files and folders on the share visible to all users during directory enumeration\.
**Note**  
Access\-based enumeration is a system that filters the enumeration of files and folders on an SMB file share based on the share's access control lists \(ACLs\)\.

1. \(Optional\) For **Allowed/denied users and groups**, choose **Add entry** and provide the list of AD users or groups that you want to allow or deny file share access\.
**Note**  
The **Allowed/denied users and groups** section appears only if **Active Directory** is selected\.  
Enter only the AD user or group name\. The domain name is implied by the membership of the gateway in the specific AD that the gateway is joined to\.  
If you don't specify valid or invalid users or groups, any authenticated AD user can export the file share\.

1. \(Optional\) For **Tags**, you add new tags or remove existing tags\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your SMB file share is created, you can see your file share settings in the file share's **Details** tab\.

------

The preceding procedure creates a Microsoft AD file share\. Anyone with domain credentials can access this file share\. To limit access to certain users and groups, see [Using Active Directory to authenticate users](managing-gateway-file.md#enable-ad-settings)\.

**Next Step**

[Mount your SMB file share on your client](using-smb-fileshare.md)