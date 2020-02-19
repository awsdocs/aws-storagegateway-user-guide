# Troubleshooting File Gateway Issues<a name="troubleshoot-logging-errors"></a>

You can configure your gateway with an Amazon CloudWatch log group when you run VMware vSphere High Availability \(HA\)\. If you do, you receive notifications about your file gateway's health status and about errors that the gateway encounters\. You can find information about these error and health notifications in CloudWatch Logs\. 

In the following sections, you can find information that can help you understand the cause of each error and health notification and how to fix issues\. 

**Topics**
+ [You Get an InaccessibleStorageClass Error](#troubleshoot-logging-errors-inaccessiblestorageclass)
+ [You Get an S3AccessDenied Error](#troubleshoot-logging-errors-s3accessdenied)
+ [You Get an InvalidObjectState Error](#troubleshoot-logging-errors-invalidobjectstate)
+ [You Get an ObjectMissing Error](#troubleshoot-logging-errors-objectmissing)
+ [You Get a Reboot Notification](#troubleshoot-reboot-notification)
+ [You Get a HardReboot Notification](#troubleshoot-hardreboot-notification)
+ [You Get a HealthCheckFailure Notification](#troubleshoot-healthcheckfailure-notification)
+ [You Get a AvailabilityMonitorTest Notification](#troubleshoot-availabilitymonitortest-notification)
+ [You Get a RoleTrustRelationshipInvalid Error](#misconfig-trust)
+ [Troubleshooting File Gateway with CloudWatch Metrics](#troubleshooting-with-cw-metrics)

## You Get an InaccessibleStorageClass Error<a name="troubleshoot-logging-errors-inaccessiblestorageclass"></a>

You can get an `InaccessibleStorageClass` error when an object has moved out of the Amazon S3 Standard storage class\. 

Here, usually your gateway encounters the error when it tries to either upload the specified object to Amazon S3 or read the object from Amazon S3\. With this error, generally the object has moved to Amazon S3 Glacier and is in either the S3 Glacier or S3 Glacier Deep Archive storage class\. 

**Action to Take**  
For either type of error, move the object from the S3 Glacier or S3 Glacier Deep Archive storage class back to S3\. 

If you move the object to S3 to fix an upload error, the file is eventually uploaded\. If you move the object to S3 to fix a read error, the file gateway's SMB or NFS client can then read the file\.

## You Get an S3AccessDenied Error<a name="troubleshoot-logging-errors-s3accessdenied"></a>

You can get an `S3AccessDenied` error for a file share's bucket access IAM role\. In this case, the bucket access IAM role that is specified by `roleArn` in the error doesn't allow the operation involved\. The operation isn't allowed because of the permissions for the objects in the directory specified by the S3 prefix\.

**Action to Take**  
Modify the S3 access policy that is attached to `roleArn` in the gateway health log to allow permissions for the S3 operation\. Make sure that the access policy allows permission for the operation that caused the error\. Also, allow permission for the directory specified in the log for `prefix`\. For information about Amazon S3 permissions, see [Specifying Permissions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in *Amazon Simple Storage Service Developer Guide\.*

These operations can cause an `S3AccessDenied` error to occur:
+ `S3HeadObject`
+ `S3GetObject`
+ `S3ListObjects`
+ `S3DeleteObject`
+ `S3PutObject`

## You Get an InvalidObjectState Error<a name="troubleshoot-logging-errors-invalidobjectstate"></a>

You can get an `InvalidObjectState` error when a writer other than the specified gateway modifies the specified file in the specified S3 bucket\. As a result, the state of the file for the gateway doesn't match its state in S3\. Any subsequent uploads of the file to S3 or retrievals of the file from S3 fail\.

**Action to Take**  
If the operation that modifies the file is `S3Upload` or `S3GetObject`, do the following to fix the error:

1. Save the latest copy of the file to the local file system of your SMB or NFS client\. You need this file copy in step 4\. If the version of the file in S3 is the latest, download that version\. You can do this using the AWS Management Console or AWS CLI\.

1. Use the console or the CLI to delete the file in S3\.

1. Use your SMB or NFS client to delete the file from the file gateway\.

1. Use your SMB or NFS client to copy the latest version of the file that you saved in step 1 to S3\. Do this through your file gateway\.

## You Get an ObjectMissing Error<a name="troubleshoot-logging-errors-objectmissing"></a>

You can get an `ObjectMissing` error when a writer other than the specified gateway deletes the specified file from the S3 bucket\. Any subsequent uploads to S3 or retrievals from S3 for the object fail\.

**Action to Take**

If the operation that modifies the file is `S3Upload` or `S3GetObject`, do the following to fix the error:

1. Save the latest copy of the file to the local file system of your SMB or NFS client\. You need this file in step 3\. 

1. Use your SMB or NFS client to delete the file from the file gateway\.

1. Use your SMB or NFS client to copy the latest version of the file that you saved in step 1\. Do this through the file gateway\.

## You Get a Reboot Notification<a name="troubleshoot-reboot-notification"></a>

You can get a reboot notification when the gateway VM is restarted\. You can restart a gateway VM by using the VM Hypervisor Management console or the Storage Gateway console\. You can also restart by using the gateway software during the gateway's maintenance cycle\.

**Action to Take**

If the time of the reboot is within 10 minutes of the gateway's configured [maintenance start time](MaintenanceManagingUpdate-common.md), this reboot is probably a normal occurrence and not a sign of any problem\. If the reboot occurred significantly outside the maintenance window, check whether the gateway was restarted manually\.

## You Get a HardReboot Notification<a name="troubleshoot-hardreboot-notification"></a>

You can get a `HardReboot` notification when the gateway VM is restarted unexpectedly\. Such a restart can be due to loss of power, a hardware failure, or another event\. For VMware gateways, a reset by vSphere High Availability Application Monitoring can trigger this event\.

**Action to Take**

When your gateway runs in such an environment, check for the presence of the `HealthCheckFailure` notification and consult the VMware events log for the VM\.

## You Get a HealthCheckFailure Notification<a name="troubleshoot-healthcheckfailure-notification"></a>

For a gateway on VMware vSphere HA, you can get a `HealthCheckFailure` notification when a health check fails and a VM restart is requested\. This event also occurs during a test to monitor availability, indicated by an `AvailabilityMonitorTest` notification\. In this case, the `HealthCheckFailure` notification is expected\.

**Note**  
This notification is for VMware gateways only\.

**Action to Take**

If this event repeatedly occurs without an `AvailabilityMonitorTest` notification, check your VM infrastructure for issues \(storage, memory, and so on\)\. If you need additional assistance, contact AWS Support\. 

## You Get a AvailabilityMonitorTest Notification<a name="troubleshoot-availabilitymonitortest-notification"></a>

You get `AvailabilityMonitorTest` notification when you [run a test](Performance.md#vmware-ha-test-failover) of the [Availability and Application Monitoring](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html) system on gateways running on a VMware vSphere HA platform\.

**Action to Take**

No action to take\.

## You Get a RoleTrustRelationshipInvalid Error<a name="misconfig-trust"></a>

You get this error when the IAM role for a file share has a misconfigured IAM trust relationship \(that is, the IAM role does not trust the Storage Gateway principal named `storagegateway.amazonaws.com`\)\. As a result, the gateway would not be able to get the credentials to execute any operations on the S3 bucket that backs the file share\.

**Action to Take**

You need to use the IAM console or IAM API to include `storagegateway.amazonaws.com` as a principal that is trusted by your file share's IAM role\. For information about IAM role, see [Tutorial: Delegate Access Across AWS Accounts Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)\.

## Troubleshooting File Gateway with CloudWatch Metrics<a name="troubleshooting-with-cw-metrics"></a>

You can find information following about actions to address issues in using Amazon CloudWatch metrics with Storage Gateway\.

**Topics**
+ [Your Gateway Reacts Slowly When Browsing Directories](#slow-gateway)
+ [Your Gateway Isn't Responding](#gateway-not-responding)
+ [Your Gateway Is Slow Transferring Data to Amazon S3](#slow-data-transfer-to-S3)
+ [Your Gateway Backup Job Fails or There Are Errors When Writing to Your Gateway](#backup-job-fails)

### Your Gateway Reacts Slowly When Browsing Directories<a name="slow-gateway"></a>

If your gateway reacts slowly when you run the ls command or browse directories, check the `IndexFetch` and `IndexEviction` CloudWatch metrics: 
+ If the `IndexFetch` metric is greater than 0 when you run an `ls` command or browse directories, your gateway started without information on the contents of the directory affected and had to access Amazon S3\. Subsequent efforts to list the contents of that directory should go faster\.
+ If the `IndexEviction` metric is greater than 0, it means that your gateway has reached the limit of what it can manage in its cache at that time\. In this case, your gateway has to free some storage space from the least recently accessed directory to list a new directory\. If this occurs frequently and there is a performance impact, contact AWS Support\. Discuss with AWS Support the contents of the related Amazon S3 bucket and recommendations to improve performance based on your use case\.

### Your Gateway Isn't Responding<a name="gateway-not-responding"></a>

If your gateway isn't responding, do the following:
+  If there was a recent reboot or software update, then check the `IOWaitPercent` metric\. This metric shows the percentage of time that the CPU is idle when there is an outstanding disk I/O request\. In some cases, this might be high \(10 or greater\) and might have risen after the server was rebooted or updated\. In these cases, then your gateway might be bottlenecked by a slow root disk as it rebuilds the index cache to RAM\. You can address this issue by using a faster physical disk for the root disk\.
+ If the `MemUsedBytes` metric is at or nearly the same as the `MemTotalBytes` metric, then your gateway is running out of available RAM\. Make sure that your gateway has at least the minimum required RAM\. If it already does, consider adding more RAM to your gateway based on your workload and use case\. 

  If the file share is SMB, the issue might also be due to the number of SMB clients connected to the file share\. To see the number of clients connected at any given time, check the `SMBV(1/2/3)Sessions` metric\. If there are many clients connected, you might need to add more RAM to your gateway\.

### Your Gateway Is Slow Transferring Data to Amazon S3<a name="slow-data-transfer-to-S3"></a>

If your gateway is slow transferring data to Amazon S3, check the `CachePercentDirty` metric\. If this metric is 80 or greater, your gateway is writing data faster to disk than it can upload the data to Amazon S3\. Consider increasing the bandwidth for upload from your gateway, adding one or more cache disks, or slowing down client writes\. 

 If the `CachePercentDirty` metric is low, check the `IoWaitPercent` metric\. If `IoWaitPercent` is greater than 10, your gateway might be bottlenecked by the speed of the local cache disk\. We recommend local solid state drive \(SSD\) disks for your cache, preferably NVM Express \(NVMe\)\. If such disks aren't available, try using multiple cache disks from separate physical disks for a performance improvement\.

### Your Gateway Backup Job Fails or There Are Errors When Writing to Your Gateway<a name="backup-job-fails"></a>

If your gateway backup job fails or there are errors when writing to your gateway, do the following:
+ If the `CachePercentDirty` metric is 90 percent or greater, your gateway can't accept new writes to disk because there is not enough available space on the cache disk\. To see how fast your gateway is uploading to Amazon S3, view the `CloudBytesUploaded` metric\. Compare that metric with the `WriteBytes` metric, which shows how fast the client is writing files to your gateway\. If your gateway is writing faster than it can upload to Amazon S3, add more cache disks to cover the size of the backup job at a minimum\. Or, increase the upload bandwidth\.
+ If a backup job fails but the `CachePercentDirty` metric is less than 80 percent, your gateway might be hitting a client\-side session timeout\. For SMB, you can increase this timeout using the PowerShell command `Set-SmbClientConfiguration -SessionTimeout 300`\. Running this command sets the timeout to 300 seconds\. For NFS, make sure that the client is mounted using a hard mount instead of a soft mount\.