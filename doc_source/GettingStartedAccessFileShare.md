# Mounting your NFS file share on your client<a name="GettingStartedAccessFileShare"></a>

Now you mount your NFS file share on a drive on your client and map it to your Amazon S3 bucket\.

**To mount a file share and map it to an Amazon S3 bucket**

1. If you are using a Microsoft Windows client, we recommend that you [create an SMB file share](https://docs.aws.amazon.com/storagegateway/latest/userguide/CreatingAnSMBFileShare.html) and access it using an SMB client that is already installed on Windows client\. If you use NFS, turn on Services for NFS in Windows\.

1. Mount your NFS file share:
   + For Linux clients, type the following command at the command prompt\.

     **sudo mount \-t nfs \-o nolock,hard *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***
   + For MacOS clients, type the following command at the command prompt\.

     **sudo mount\_nfs \-o vers=3,nolock,rwsize=65536,hard \-v *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***
   + For Windows clients, type the following command at the command prompt\.

     **mount –o nolock \-o mtype=hard *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[Drive letter on your windows client\]***

   For example, suppose that on a Windows client your VM's IP address is 123\.123\.1\.2 and your Amazon S3 bucket name is `test-bucket`\. Suppose also that you want to map to drive T\. In this case, your command looks like the following\.

   **mount \-o nolock \-o mtype=hard 123\.123\.1\.2:/test\-bucket T:**
**Note**  
When mounting file shares, be aware of the following:  
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a file gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a file gateway\.
You might need to remount your file share after a reboot of your client\.
By default Windows uses a soft mount for mounting your NFS share\. Soft mounts time out more easily when there are connection issues\. We recommend using a hard mount because a hard mount is safer and better preserves your data\. The soft mount command omits the **\-o mtype=hard** switch\. The Windows hard mount command uses the `-o mtype=hard` switch\.
If you are using Windows clients, check your `mount` options after mounting by running the `mount` command with no options\. The response should that confirm the file share was mounted using the latest options you provided\. It also should confirm that you are not using cached old entries, which take at least 60 seconds to clear\.

**Next Step**

[Testing your file gateway](GettingStartedTestFileShare.md)