# Test your S3 File<a name="GettingStartedTestFileShare"></a>

 You can copy files and folders to your mapped drive\. The files automatically upload to your Amazon S3 bucket\.

**To upload files from your Windows client to Amazon S3**

1. On your Windows client, navigate to the drive that you mounted your file share on\. The name of your drive is preceded by the name of your S3 bucket\.

1. Copy files or a folder to the drive\.

1. On the Amazon S3 Management Console, navigate to your mapped bucket\. You should see the files and folders that you copied in the Amazon S3 bucket that you specified\.

   You can see the file share that you created in the **File shares** tab in the AWS Storage Gateway Management Console\.

Your NFS or SMB client can write, read, delete, rename, and truncate files\.

**Note**  
File gateways don't support creating hard or symbolic links on a file share\.

Keep in mind these points about how file gateways work with S3:
+ Reads are served from a read\-through cache\. In other words, if data isn't available, it's fetched from S3 and added to the cache\.
+ Writes are sent to S3 through optimized multipart uploads by using a write\-back cache\.
+ Read and writes are optimized so that only the parts that are requested or changed are transferred over the network\.
+ Deletes remove objects from S3\.
+ Directories are managed as folder objects in S3, using the same syntax as in the Amazon S3 console\. You can rename empty directories\.
+ Recursive file system operation performance \(for example `ls –l`\) depends on the number of objects in your bucket\.

**Next Step**

[Where do I go from here?](GettingStartedWhatsNextStep3File.md)