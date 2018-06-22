# Working with File Shares on a Bucket with Pre\-exisiting Objects<a name="FileSharePrexistingObjects"></a>

You can export a file share on an Amazon S3 bucket with objects created outside of the file gateway using either NFS or SMB\. Standard POSIX access and permissions are used in the file share\. When files are written back to the Amazon S3 bucket they assume the properties and access rights that the person saving the files give them\. Objects can be uploaded to an S3 bucket at any time\. For the file share to display these newly added objects as files, you need to [refresh the cache](https://docs.aws.amazon.com/storagegateway/latest/userguide/managing-gateway-file.html#refresh-cache) first\.

**Note**  
Be sure to read the *Can I have multiple writers to my Amazon S3 bucket?* in the [FAQ](https://aws.amazon.com/storagegateway/faqs/) section\. This practice is not recommended\.

To assign metadata defaults to objects accessed using **NFS**, refer to the section *Editing Metadata Defaults* iin the [Managing Your File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/managing-gateway-file.html) topic\.

For **SMB** you can export a share using AD or guest access on an Amazon S3 bucket with preexisting objects\. Objects in the bucket created outside of the gateway display as files in the SMB file system when they are accessed by your SMB clients\. SMB file share files inherits ownership and permissions from the root ACL\. The ower=smbguest, file permissions are 666 and directory permissions are 777 when there are no parent directories\. In the case when there are directories into which objects were previously populated the object inherits the POSIX metadata of those parent directories\. 