# Creating an SMB File Share<a name="CreatingAnSMBFileShare"></a>

Creating an SMB accessible file share is a *two\-step process*\. Before you create an SMB file share you need to configure your file gateway **SMB Settings** for either Active Directory or Guest access\. A file share can provide one type of SMB access only\. After setting the authentication methods you create your file share\.

**Note**  
An SMB file share will not operate correctly without the requisite ports open in your security group\. For more information, see [Port Requirements](Resource_Ports.md)\.

**To configure your SMB file share for Active Directory access**

1. Sign in to your console and on the **Gateway** page enable the check box next to the file gateway you wish to join to a domain\.

1. Select **Actions**, choose then **Edit SMB Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SMBFileShareActionsMenu.png)

   The **Edit SMB dialog box** appears as shown in the figure below\.

1. For *Active Directory authentication* enable the **Join Domain** check box,

1. Enter your fully qualified domain name into the **Domain name** text field\.
**Note**  
The AWS Directory Service allows you to create a hosted Active Directory domain service in the AWS cloud if you require that option\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Edit-SMB-Setttings-db.png)

1. Enter your account name into the **Domain name** text box\. This account must have the privilege of joining a server to a domain\.

1. Type in your acccount password into the **Domain password** text box\.

1. Then select **Save** to complete SMB file share Active Directory authentication\.

   A message at the top of the Gateways section of your console indicates that your gateway successfully joined your AD domain\.

**To configure your SMB file share for Guest access**

1. Sign in to your console and on the **Gateway** page enable the check box next to the file gateway you wish to join to a domain\.

1. Select **Actions**, choose then **Edit SMB Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SMBFileShareActionsMenu.png)

   The **Edit SMB dialog box** appears as shown in the figure below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Edit-SMB-Setttings-db.png)

1. Enable *Guest access* for your SMB file share enable **Set guest password**\.
**Note**  
It is not necessary for a file gateway to be part of an AD domain if only guest access is provided\. A file gateway that is a member of your AD domain can also be used to create file shares with guest access\.

1. In Guest password enter a password that meets your organization’s security requirements\. 

1. Choose **Save** to complete SMB file share Guest access authentication\.

   A message at the top of the Gateways section of your console indicates that your gateway now allows guest access\.

In the next procedure you will create an SMB file share with either Active Directory or Guest access using the **Create File Share** wizard\. You should have completed the SMB file share settings \(above\) for your file gateway before peforming the steps below\. 

**To create an SMB file share**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Shares**, select the file gateway you wish to use, and then choose **Create file share**\. You can also choose **Create file share** from the Gateway tab\.

   The **Create file share** wizard appears with the **Configure file share settings** page open as shown below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-step.png)

1. For **Amazon S3 bucket name**, provide a name for an existing Amazon S3 bucket for your gateway to store your files in and retrieve your files to\.

   The Create file share wizard cannot create an S3 bucket automatically\. You will need to create the bucket beforehand in the AWS S3 service\.

1. Select **Server Message Block \(SMB\)** in the **Access Objects using** section\.

1. If you have already selected the file gateway prior to starting the wizard its name will be populated into the Gateway list box\. If not, then select the file gateway you wish to use from the list box, then select **Next**\.

   The **Create file share** wizard **Storage** step appears as shown below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-storage-db.png)

1. For **Storage class for new objects**, choose a storage class to use for new objects created in your Amazon S3 bucket:
   + Choose **S3 Standard** to store your frequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 Standard\-IA** to store your infrequently accessed object data redundantly in multiple Availability Zones that are geographically separated\.
   + Choose **S3 One Zone\-IA** to store your infrequently accessed object data in a single Availability Zone\.

   For more information, see [Storage Classes](http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Object metadata**, choose the metadata you want to use:
   + Choose **Guess MIME type** to enable guessing of the MIME type for uploaded objects based on file extensions\.
   + Choose **Give bucket owner full control** to give full control to the owner of the S3 bucket that maps to the file NFS file share\. For more information on using your file share to access objects in a bucket owned by another account, see [Using a File Share for Cross\-Account Access](managing-gateway-file.md#cross-account-access)\.
   + Choose **Enable requester pays** if you are using this file share on a bucket that requires the requester or reader instead of bucket owner to pay for access charges\. For more information, see [Requester Pays Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html)\.

1. For **Access to your bucket**, choose the AWS Identity and Access Management \(IAM\) role that you want your gateway to use to access your Amazon S3 bucket\. This role allows the gateway to access your S3 bucket\. A file gateway can create a new IAM role and access policy on your behalf\. Or, if you have an IAM role you want to use, you can specify it in the **IAM role** box and set up the access policy manually\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\. For information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

1. Choose **Next** to view the **Review** configuration settings for your SMB file share\.as shown in the figure below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-review-db.png)  
  


1. *Active Directory access* is the default authentication method\. For AD authentication check that Active Directory is shown in the **Select authentication method** text box\.
**Note**  
For Active Directory access your file gateway must be joined to a domain\.  
For Guest access you must have set a guest access password\.

1. If you want to change to *Guest access* choose **Edit** in the **SMB server configuration** section, and then select **Guest acess** from the drop down menu\.

1. Choose the **Read\-write** \(the default\) or **Read\-only** option\. Choose **Close** to enforce your authentication settings\.

1. Review your file share configuration settings, and then choose **Create file share**\.

   After your SMB file share is created, you can see your file share settings in the file share's Details tab\. A green banner is posted that shows your new file share was successfully created as shown in the figure below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Create-file-share-success.png)  
  


The procedure above creates an Active Directory file share where anyone with domain credentials can access the file share\. To limit access to certain users and groups see [Editing Access Settings for Your SMB File Share](managing-gateway-file.md#enable-ad-settings) 

**Next Step**

[Mounting Your SMB File Share on Your Client](using-smb-fileshare.md)
