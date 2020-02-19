# Creating an SMB File Share<a name="CreatingAnSMBFileShare"></a>

Before you create an SMB file share, make sure that you configure security settings and SMB settings for your file gateway\. You also configure either Microsoft Active Directory \(AD\) or guest access for authentication\. A file share provides one type of SMB access only\.

**Note**  
An SMB file share doesn't operate correctly without the needed ports open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and on the **Gateway** page, choose the box next to the file gateway that you want to join to a domain\.

1. For **Actions**, choose **Edit SMB settings**\.

At this point, configure settings for your file gateway:
+ Configure security settings\.
+ Configure Active Directory settings\.
+ Configure guest access\.

Find details on how to configure these settings following\.

**To configure security settings**

1. In the SMB security settings section, choose **Set security level**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/security-strategy.png)

1. For **Security level**, choose one of the following:
**Note**  
This setting is called SMBSecurityStrategy in the API Reference\.  
A higher security level can affect performance\.
   + **Enforce encryption** – if you choose this option, file gateway only allows connections from SMBv3 clients that have encryption enabled\. This option is highly recommended for environments that handle sensitive data\. This option works with SMB clients on Microsoft Windows 8, Windows Server 2012, or newer\. 
   + **Enforce signing** – if you choose this option, file gateway only allows connections from SMBv2 or SMBv3 clients that have signing enabled\. This option works with SMB clients on Microsoft Windows Vista, Windows Server 2008, or newer\. 
   + **Client negotiated** – if you choose this option, requests are established based on what is negotiated by the client\. This option is recommended when you want to maximize compatibility across different clients in your environment\.
**Note**  
For gateways activated before June 20, 2019, the default security level is **Client negotiated**\.  
For gateways activated on June 20, 2019 and later, the default security level is **Enforce encryption**\.

**To configure your SMB file share for Microsoft Active Directory access**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and on the **Gateway** page, choose the box next to the file gateway that you want to join to a domain\.

1. For **Actions**, choose **Edit SMB settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/smb-join-domain.png)

1. In the **Active Directory settings** section, for **Domain name**, provide the domain that you want the gateway to join\. You can join a domain by using its IP address or its organizational unit\. An *organizational unit* is an Active Directory subdivision that can hold users, groups, computers, and other organizational units\.
**Note**  
If your gateway can't join an Active Directory directory, try joining with the directory's IP address by using the [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html) API operation\.
**Note**  
**Active Directory status** shows **Detached** when a gateway has never joined a domain\.

1. For **Domain name**, enter your fully qualified domain name\.
**Note**  
You can use the [AWS Directory Service](http://aws.amazon.com/directoryservice/) to create a hosted Microsoft Active Directory domain service in the AWS Cloud\.

1. For **Domain user**, enter your account name\. Your account must be able to join a server to a domain\.

1. For **Domain password**, enter your account password\.

1. For **Organizational unit**, enter your organizational unit\.

1. For **Domain controllers**, enter a comma\-separated list of Internet Protocol version 4 \(IPv4\) addresses, NetBIOS names, or hostnames of your domain server\.

**To configure your SMB file share for guest access**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and on the **Gateway** page, choose the box next to the file gateway that you want to use for your guest file share\.

1. For **Actions**, choose **Edit SMB settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/smb-guest-access.png)

1. Choose **Set guest password** to enable guest access for your SMB file share\. 
**Note**  
If you provide only guest access, your file gateway doesn't have to be part of an AD domain\. You can also use a file gateway that is a member of your Microsoft AD domain to create file shares with guest access\.

1. For Guest password, enter a password that meets your organization's security requirements\. 

1. Choose **Save** to complete the authentication\.

   A message at the top of the **Gateways** section of your console should appear, saying that your gateway `Successfully joined domain`\.

   If the banner displays the message `Invalid domain name/DNS name cannot be resolved`, the correct endpoint wasn't found\. You might also see the error `Invalid users/Invalid password`, an authentication failure that means that your logon was not recognized by the domain service\.

   The error message `The gateway cannot connect to the specified domain` can indicate that the quota of users has been exhausted, in other words there are no more users in the quota\. The default limit allows each user to join up to 10 systems to a domain\. This error can also appear if the user that tried to connect didn't have administrator privileges\.

   The error message `The specified request timed out` might indicate that there is a problem with your firewall rules not allowing access to the domain\.

In the next procedure, you create an SMB file share with either Microsoft Active Directory or guest access\. Make sure that you define the SMB file share settings for your file gateway before performing the following steps\.

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Shares**, choose the file gateway that you want to use, and then choose **Create file share**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-step.png)

1. On the **Configure file share settings** page, for **Amazon S3 bucket name**, provide a name for an existing Amazon S3 bucket\. You use this bucket for your gateway to store files in and retrieve 

1. For **Access Objects using**, choose **Server Message Block \(SMB\)**\.

1. For **Gateway**, make sure that your gateway is chosen, and then choose **Next**\.

   The **Configure how files are stored** in Amazon S3 page appears, as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-storage-db.png)

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\.

   For more information, see [Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file SMB file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your gateway to use to access your Amazon S3 bucket\. This role allows the gateway to access your S3 bucket\. A file gateway can create a new IAM role and access policy on your behalf\. Or, if you have an IAM role you want to use, you can specify it in the **IAM role** box and set up the access policy manually\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\. For information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your file share\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file share\. 

1. Choose **Next** to review configuration settings for your SMB file share, as shown in the figure following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/gateway-settings-review-db.png)  
  


1. For Microsoft AD authentication, make sure that **Active Directory** appears for **Select authentication method**\. Microsoft AD access is the default authentication method\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/file-share-settings-review-db.png)  
  

**Note**  
For Microsoft AD access, your file gateway must be joined to a domain\.  
For guest access, you must have set a guest access password\.   
Both access types are available at the same time\.

1. For Export as, choose **Read\-write** \(the default\) or **Read\-only**\. Choose **Close** to enforce your authentication settings\.

1. For **File/directory access controlled by**, choose one of the following:
   + Choose **Windows Access Control List** to set fine\-grained permissions on files and folders in your SMB file share\. For more information, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.
   + Choose **POSIX permissions** to use POSIX permissions to control access to files and directories that are stored through an NFS or SMB file share\.

1. \(Optional\) For **Admin users/groups**, enter a comma\-separated list of AD users and groups\. You do this if you want the admin user to have privileges to update ACLs on all files and folders in the file share\. These users and groups then have administrator rights to the file share\. A group must be prefixed with the @ character, for example `@group1`\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your SMB file share is created, you can see your file share settings in its **Details** tab\.

The preceding procedure creates a Microsoft Active Directory file share\. Anyone with domain credentials can access this file share\. To limit access to certain users and groups, see [Using Active Directory to Authenticate Users](managing-gateway-file.md#enable-ad-settings)\.

**Next Step**

[Mounting Your SMB File Share on Your Client](using-smb-fileshare.md)