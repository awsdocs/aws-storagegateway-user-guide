# Best practices for recovering your data<a name="recover-data-from-gateway"></a>

Although it is rare, your gateway might encounter an unrecoverable failure\. Such a failure can occur in your virtual machine \(VM\), the gateway itself, the local storage, or elsewhere\. If a failure occurs, we recommend that you follow the instructions in the appropriate section following to recover your data\.

**Important**  
Storage Gateway doesn’t support recovering a gateway VM from a snapshot that is created by your hypervisor or from your Amazon EC2 Amazon Machine Image \(AMI\)\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway using the instructions following\.

**Topics**
+ [Recovering from an unexpected virtual machine shutdown](#recover-from-gateway-shutdown)
+ [Recovering your data from a malfunctioning cache disk](#recover-from-cahe-disk)
+ [Recovering your data from an inaccessible data center](#disaster-recovery)

## Recovering from an unexpected virtual machine shutdown<a name="recover-from-gateway-shutdown"></a>

If your VM shuts down unexpectedly, for example during a power outage, your gateway becomes unreachable\. When power and network connectivity are restored, your gateway becomes reachable and starts to function normally\. Following are some steps you can take at that point to help recover your data:
+ If an outage causes network connectivity issues, you can troubleshoot the issue\. For information about how to test network connectivity, see [Testing your S3 File gateway connection to the internet](manage-on-premises-fgw.md#MaintenanceTestGatewayConnectivity-fgw)\.
+ If your gateway malfunctions and issues occur with your volumes or tapes as a result of an unexpected shutdown, you can recover your data\. For information about how to recover your data, see the sections following that apply to your scenario\.

## Recovering your data from a malfunctioning cache disk<a name="recover-from-cahe-disk"></a>

If your cache disk encounters a failure, we recommend you use the following steps to recover your data depending on your situation:
+ If the malfunction occurred because a cache disk was removed from your host, shut down the gateway, re\-add the disk, and restart the gateway\.
+ If the cache disk is corrupted or not accessible, shut down the gateway, reset the cache disk, reconfigure the disk for cache storage, and restart the gateway\.

For detailed information, see [Recovering your data from a malfunctioning cache disk](#recover-from-cahe-disk)\.

## Recovering your data from an inaccessible data center<a name="disaster-recovery"></a>

If your gateway or data center becomes inaccessible for some reason, you can recover your data to another gateway in a different data center or recover to a gateway hosted on an Amazon EC2 instance\. If you don't have access to another data center, we recommend creating the gateway on an Amazon EC2 instance\. The steps you follow depends on the gateway type you are covering the data from\.

**To recover data from a file gateway in an inaccessible data center**

For file gateway, you map a new file share to the Amazon S3 bucket that contains the data you want to recover\.

1. Create and activate a new file gateway on an Amazon EC2 host\. For more information, see [Deploying a file gateway on an Amazon EC2 host](ec2-gateway-file.md)\.

1. Create a new file share on the EC2 gateway you created\. For more information, see [Create a file share](https://docs.aws.amazon.com/filegateway/latest/files3/GettingStartedCreateFileShare.html)\.

1. Mount your file share on your client and map it to the S3 bucket that contains the data that you want to recover\. For more information, see [Mount and use your file share](https://docs.aws.amazon.com/filegateway/latest/files3/getting-started-use-fileshare.html)\.