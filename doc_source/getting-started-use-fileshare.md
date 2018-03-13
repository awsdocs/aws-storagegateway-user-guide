# Using Your File Share<a name="getting-started-use-fileshare"></a>

Following, you can find instructions about how to mount your file share on your client, use your share, test your file gateway, and clean up resources as needed\. Your file share accepts connections from any NFS client\. For more information, see [Supported NFS Clients for a File Gateway](Requirements.md#requirements-nfs-clients)\.


+ [Mounting Your File Share on Your Client](#GettingStartedAccessFileShare)
+ [Testing Your File Gateway](#GettingStartedTestFileShare)
+ [Where Do I Go from Here?](#GettingStartedWhatsNextStep3File)

## Mounting Your File Share on Your Client<a name="GettingStartedAccessFileShare"></a>

Now you mount the file share on a drive on your client and map it to your Amazon S3 bucket\.

**To mount a file share and map it to an Amazon S3 bucket**

1. If you are using a Windows client, turn on Services for NFS\.

1. Mount your file share:

   + For Windows clients, type the following command at the command prompt\.

     **mount –o nolock *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[Drive letter on your windows client\]***

   + For Linux clients, type the following command at the command prompt\.

     **sudo mount \-t nfs \-o nolock *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***

   + For MacOS clients, type the following command at the command prompt\.

     **sudo mount\_nfs \-o vers=3,nolock,rwsize=65536 \-v *\[Your gateway VM IP address\]*:/*\[S3 bucket name\]* *\[mount path on your client\]***

   For example, suppose that on a Windows client your VM's IP address is 123\.456\.1\.2 and your Amazon S3 bucket name is `test-bucket`\. Suppose also that you want to map to drive T\. In this case, your command looks like the following:

   **mount –o nolock 123\.456\.1\.2:/test\-bucket T:**
**Note**  
When mounting file shares, be aware of the following:  
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a file gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a file gateway\.
You might need to remount your file share after a reboot of your client\.

## Testing Your File Gateway<a name="GettingStartedTestFileShare"></a>

 You can copy files and folders to your mapped drive\. The files automatically upload to your Amazon S3 bucket\.

**To upload files from your windows client to Amazon S3**

1. On your Windows client, navigate to the drive that you mounted your file share on\. The name of your drive is preceded by the name of your S3 bucket\.

1. Copy files or a folder to the drive\.

1. On the Amazon S3 Management Console, navigate to your mapped bucket\. You should see the files and folders that you copied in the Amazon S3 bucket that you specified\. 

   You can see the file share that you created in the **File shares** tab in the AWS Storage Gateway Management Console\.

Your NFS client can write, read, delete, rename, and truncate files\. 

**Note**  
File gateways don't support creating hard or symbolic links on a file share\.

Keep in mind these points about how file gateways work with S3:

+ Reads are served from a read\-through cache\. In other words, if data isn't available, it's fetched from S3 and added to the cache\. 

+ Writes are sent to S3 through optimized multipart uploads by using a write\-back cache\. 

+ Read and writes are optimized so that only the parts that are requested or changed are transferred over the network\. 

+ Deletes remove objects from S3\. 

+ Directories are managed as folder objects in S3, using the same syntax as in the Amazon S3 console\. You can rename empty directories\. 

+ Recursive file system operation performance \(for example `ls –l`\) depends on the number of objects in your bucket\. 

## Where Do I Go from Here?<a name="GettingStartedWhatsNextStep3File"></a>

In the preceding sections, you created and started using a file gateway, including mounting a file share and testing your setup\. 

Other sections of this guide include information about how to do the following:

+ To manage your file gateway, see [Managing Your File Gateway](managing-gateway-file.md)\.

+ To optimize your file gateway, see [Optimizing Gateway Performance](Optimizing-common.md)\.

+ To troubleshoot gateway problems, see [Troubleshooting Your Gateway](Troubleshooting-common.md)\.

+ To learn about Storage Gateway metrics and how you can monitor how your gateway performs, see [Monitoring Your Gateway and Resources](Main_monitoring-gateways-common.md)\.

### Cleaning Up Resources You Don't Need<a name="cleanup-file"></a>

If you created your gateway as an example exercise or a test, consider cleaning up to avoid incurring unexpected or unnecessary charges\. 

**To clean up resources you don't need**

1. Delete any snapshots\. For instructions, see [Deleting a Snapshot](managing-volumes.md#DeletingASnapshot)\.

1. Unless you plan to continue using the gateway, delete it\. For more information, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

1. Delete the AWS Storage Gateway VM from your on\-premises host\. If you created your gateway on an Amazon EC2 instance, terminate the instance\. 