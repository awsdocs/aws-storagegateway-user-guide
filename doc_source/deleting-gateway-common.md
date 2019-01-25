# Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources<a name="deleting-gateway-common"></a>

If you don't plan to continue using your gateway, consider deleting the gateway and its associated resources\. Removing resources avoids incurring charges for resources you don't plan to continue using and helps reduce your monthly bill\. 

When you delete a gateway, it no longer appears on the AWS Storage Gateway Management Console and its iSCSI connection to the initiator is closed\. The procedure for deleting a gateway is the same for all gateway types; however, depending on the type of gateway you want to delete and the host it is deployed on, you follow specific instructions to remove associated resources\. 

You can delete a gateway using the Storage Gateway console or programmatically\. You can find information following about how to delete a gateway using the Storage Gateway console\. If you want to programmatically delete your gateway, see *[AWS Storage Gateway API Reference](https://docs.aws.amazon.com/storagegateway/latest/APIReference/)\.* 

**Topics**
+ [Deleting Your Gateway by Using the AWS Storage Gateway Console](#delete-gateway-procedure)
+ [Removing Resources from a Gateway Deployed On\-Premises](#remove-resources-onpremise)
+ [Removing Resources from a Gateway Deployed on an Amazon EC2 Instance](#EC2GatewayCleanup)

## Deleting Your Gateway by Using the AWS Storage Gateway Console<a name="delete-gateway-procedure"></a>

The procedure for deleting a gateway is the same for all gateway types\. However, depending on the type of gateway you want to delete and the host the gateway is deployed on, you might have to perform additional tasks to remove resources associated with the gateway\. Removing these resources helps you avoid paying for resources you don't plan to use\. 

**Note**  
For gateways deployed on a Amazon Elastic Compute Cloud \(Amazon EC2\) instance, the instance continues to exist until you delete it\.  
For gateways deployed on a virtual machine \(VM\), after you delete your gateway the gateway VM still exists in your virtualization environment\. To remove the VM, use the VMware vSphere client or Microsoft Hyper\-V Manager to connect to the host and remove the VM\. Note that you can't reuse the deleted gateway's VM to activate a new gateway\.

**To delete a gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway you want to delete\.

1. On the **Actions** menu, choose **Delete gateway**\.

1. 
**Important**  
Before you do this step, be sure that there are no applications currently writing to the gateway's volumes\. If you delete the gateway while it is in use, data loss can occur\.
**Warning**  
When a gateway is deleted, there is no way to get it back\.

   In the confirmation dialog box that appears, select the check box to confirm your deletion\. Make sure the gateway ID listed specifies the gateway you want to delete\. and then choose **Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/delete-gateway.png)

**Important**  
You no longer pay software charges after you delete a gateway, but resources such as virtual tapes, Amazon Elastic Block Store \(Amazon EBS\) snapshots, and Amazon EC2 instances persist\. You will continue to be billed for these resources\. You can choose to remove Amazon EC2 instances and Amazon EBS snapshots by canceling your Amazon EC2 subscription\. If you want to keep your Amazon EC2 subscription, you can delete your Amazon EBS snapshots using the Amazon EC2 console\.

## Removing Resources from a Gateway Deployed On\-Premises<a name="remove-resources-onpremise"></a>

You can use the instructions following to remove resources from a gateway that is deployed on\-premises\.

### Removing Resources from a Volume Gateway Deployed on a VM<a name="MaintenanceDeleteGateway-common"></a>

If the gateway you want to delete are deployed on a virtual machine \(VM\), we suggest that you take the following actions to clean up resources: 
+ Delete the gateway\. For instructions, see [Deleting Your Gateway by Using the AWS Storage Gateway Console](#delete-gateway-procedure)\.
+ Delete all Amazon EBS snapshots you don't need\. For instructions, see [Deleting an Amazon EBS Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-snapshot.html) in the *Amazon EC2 User Guide for Linux Instances*\.

### Removing Resources from a Tape Gateway Deployed on a VM<a name="MaintenanceDeleteGateway-vtl-common"></a>

When you delete a gateway–virtual tape library \(VTL\), you perform additional cleanup steps before and after you delete the gateway\. These additional steps help you remove resources you don't need so you don't continue to pay for them\. 

If the tape gateway you want to delete is deployed on a virtual machine \(VM\), we suggest that you take the following actions to clean up resources\.

**Important**  
Before you delete a tape gateway, you must cancel all tape retrieval operations and eject all retrieved tapes\.  
After you have deleted the tape gateway, you must remove any resources associated with the tape gateway that you don't need to avoid paying for those resources\.

When you delete a tape gateway, you can encounter one of two scenarios\.
+ ****The tape gateway is connected to AWS** –** If the tape gateway is connected to AWS and you delete the gateway, the iSCSI targets associated with the gateway \(that is, the virtual tape drives and media changer\) will no longer be available\. 
+ ****The tape gateway is not connected to AWS** –** If the tape gateway is not connected to AWS, for example if the underlying VM is turned off or your network is down, then you cannot delete the gateway\. If you attempt to do so, after your environment is back up and running you might have a tape gateway running on\-premises with available iSCSI targets\. However, no tape gateway data will be uploaded to, or downloaded from, AWS\. 

If the tape gateway you want to delete is not functioning, you must first disable it before you delete it, as described following: 
+ To delete tapes that have the RETRIEVED status from the library, eject the tape using your backup software\. For instructions, see [Archiving the Tape](backup_netbackup-vtl.md#GettingStarted-archiving-tapes-vtl)\.

After disabling the tape gateway and deleting tapes, you can delete the tape gateway\. For instructions on how to delete a gateway, see [Deleting Your Gateway by Using the AWS Storage Gateway Console](#delete-gateway-procedure)\.

If you have tapes archived, those tapes remain and you continue to pay for storage until you delete them\. For instruction on how to delete tapes from a archive\. see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\. 

**Important**  
You are charged for a minimum of 90 days storage for virtual tapes in a archive\. If you retrieve a virtual tape that has been stored in the archive for less than 90 days, you are still charged for 90 days storage\. 

## Removing Resources from a Gateway Deployed on an Amazon EC2 Instance<a name="EC2GatewayCleanup"></a>

If you want to delete a gateway that you deployed on an Amazon EC2 instance, we recommend that you clean up the AWS resources that were used with the gateway, specifically the Amazon EC2 instance, any Amazon EBS volumes, and also tapes if you deployed a tape gateway\. Doing so helps avoid unintended usage charges\.

### Removing Resources from Your Cached Volumes Deployed on Amazon EC2<a name="ec2-delete-cached"></a>

If you deployed a gateway with cached volumes on EC2, we suggest that you take the following actions to delete your gateway and clean up its resources:

1. In the Storage Gateway console, delete the gateway as shown in [Deleting Your Gateway by Using the AWS Storage Gateway Console](#delete-gateway-procedure)\.

1. In the Amazon EC2 console, stop your EC2 instance if you plan on using the instance again\. Otherwise, terminate the instance\. If you plan on deleting volumes, make note of the block devices that are attached to the instance and the devices' identifiers before terminating the instance\. You will need these to identify the volumes you want to delete\. 

1. In the Amazon EC2 console, remove all Amazon EBS volumes that are attached to the instance if you don't plan on using them again\. For more information, see [Clean Up Your Instance and Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-clean-up-your-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

### Removing Resources from Your Tape Gateway Deployed on Amazon EC2<a name="ec2-delete-vtl"></a>

If you deployed a tape gateway, we suggest that you take the following actions to delete your gateway and clean up its resources:

1. Delete all virtual tapes that you have retrieved to your tape gateway\. For more information, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\.

1. Delete all virtual tapes from the tape library\. For more information, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\.

1. Delete the tape gateway\. For more information, see [Deleting Your Gateway by Using the AWS Storage Gateway Console](#delete-gateway-procedure)\.

1. Terminate all Amazon EC2 instances, and delete all Amazon EBS volumes\. For more information, see [Clean Up Your Instance and Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-clean-up-your-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Delete all archived virtual tapes\. For more information, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\.
**Important**  
You are charged for a minimum of 90 days storage for virtual tapes in the archive\. If you retrieve a virtual tape that has been stored in the archive for less than 90 days, you are still charged for 90 days storage\. 