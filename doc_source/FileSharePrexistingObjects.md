# Working with file shares on a bucket with pre\-exisiting objects<a name="FileSharePrexistingObjects"></a>

You can export a file share on an Amazon S3 bucket with objects created outside of the file gateway using either NFS or SMB\. Objects in the bucket that were created outside of the gateway display as files in either the NFS or SMB file system when your file system clients access them\. Standard Portable Operating System Interface \(POSIX\) access and permissions are used in the file share\. When you write files back to an Amazon S3 bucket, the files assume the properties and access rights that you give them\.

You can upload objects to an S3 bucket at any time\. For the file share to display these newly added objects as files, you need to refresh the S3 bucket\. For more information, see [Refreshing objects in your Amazon S3 bucket](managing-gateway-file.md#refresh-cache)\.

**Note**  
We don't recommend having multiple writers for one Amazon S3 bucket\. If you do, be sure to read the section "Can I have multiple writers to my Amazon S3 bucket?" in the [Storage Gateway FAQ](https://aws.amazon.com/storagegateway/faqs/)\. 

To assign metadata defaults to objects accessed using NFS, see Editing Metadata Defaults in [Managing your Amazon S3 File Gateway](managing-gateway-file.md)\.

For SMB, you can export a share using Microsoft AD or guest access for an Amazon S3 bucket with pre\-existing objects\. Objects exported through an SMB file share inherits POSIX ownership and permissions from the parent directory right above it\. For objects under the root folder, root Access Control Lists \(ACL\) are inherited\. For Root ACL, the owner is `smbguest` and the permissions for files are `666` and the directories are `777`\. This applies to all forms of authenticated access \(Microsoft AD and guest\)\.