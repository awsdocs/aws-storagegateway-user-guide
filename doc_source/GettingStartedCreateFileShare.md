# Creating a File Share<a name="GettingStartedCreateFileShare"></a>

**Topics**
+ [Creating an NFS File Share](CreatingAnNFSFileShare.md)
+ [Creating an SMB File Share](CreatingAnSMBFileShare.md)

In this section, you can find instructions about how to create a file share\. You can create a file share that can be accessed using either the NFS or SMB protocol\. 

For **NFS**:
+ There is one type of NFS file share\. When you create an NFS share, by default, anyone who has access to the NFS server can access the NFS file share\. You can limit access to clients by IP address\.

For **SMB**, you can have one of three different modes of authentication:
+ A file share with Active Directory access\. Any authenticated AD user will get access to this file share type\.
+ An SMB file share with limited access\. Only certain domain users and groups that you speciffy are allowed access\.
+ An SMB file share with guest access\. Any users who can provide the guest password will get access to this file share\.

A file gateway can host one or more file shares of different types\. You can have multiple NFS and/or SMB file shares on a file gateway, but only one form of SMB authentication is allowed per file gateway\.

**Important**  
To create a file share, a file gateway requires you to activate AWS Security Token Service \(AWS STS\)\. Make sure that AWS STS is activated in the AWS Region that you are creating your file gateway in\. If AWS STS is not activated in that AWS Region, activate it\. For information about how to activate AWS STS, see [Activating and Deactivating AWS STS in an AWS Region](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt objects that the file gateway stores in Amazon S3\. Currently, you can do this by using the AWS Storage Gateway API Reference\. For more information, see the [API Reference](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_Operations.html) for instructions\. 