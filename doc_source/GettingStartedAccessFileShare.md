# Mounting Your NFS File Share on Your Client<a name="GettingStartedAccessFileShare"></a>

Now you mount your NFS file share on a drive on your client and map it to your Amazon S3 bucket\.

**To mount a file share and map it to an Amazon S3 bucket**

1. If you are using a Microsoft Windows client, turn on Services for NFS in Windows\.

1. Mount your NFS file share:
   + For Windows clients, type the following command at the command prompt\.

     **mount –o nolock \-o mtype=hard *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[Drive letter on your windows client\]***
   + For Linux clients, type the following command at the command prompt\.

     **sudo mount \-t nfs \-o nolock *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***
   + For MacOS clients, type the following command at the command prompt\.

     **sudo mount\_nfs \-o vers=3,nolock,rwsize=65536 \-v *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***

   For example, suppose that on a Windows client your VM's IP address is 123\.456\.1\.2 and your Amazon S3 bucket name is `test-bucket`\. Suppose also that you want to map to drive T\. In this case, your command looks like the following\.

   **mount –o nolock 123\.456\.1\.2:/test\-bucket T:**
**Note**  
When mounting file shares, be aware of the following:  
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a file gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a file gateway\.
You might need to remount your file share after a reboot of your client\.
By default Windows uses a soft mount for mounting your NFS share\. Soft mounts time out more easily when there are connection issues\. We recommend using a hard mount because a hard mount is safer and better preserves your data\. The soft mount command omits the **\-o mtype=hard** switch\. 

**Next Step**

[Testing Your File Gateway](GettingStartedTestFileShare.md)