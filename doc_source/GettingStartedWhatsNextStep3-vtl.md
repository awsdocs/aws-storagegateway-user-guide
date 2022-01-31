--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Where Do I Go from Here?<a name="GettingStartedWhatsNextStep3-vtl"></a>

After your Tape Gateway is in production, you can perform several maintenance tasks, such as adding and removing tapes, monitoring and optimizing gateway performance, and troubleshooting\. For general information about these management tasks, see [Managing Your Gateway](managing-gateway-common.md)\.

You can perform some of the Tape Gateway maintenance tasks on the AWS Management Console, such as configuring your gateway's bandwidth rate limits and managing gateway software updates\. If your Tape Gateway is deployed on\-premises, you can perform some maintenance tasks on the gateway's local console\. These include routing your Tape Gateway through a proxy and configuring your gateway to use a static IP address\. If you are running your gateway as an Amazon EC2 instance, you can perform specific maintenance tasks on the Amazon EC2 console, such as adding and removing Amazon EBS volumes\. For more information on maintaining your Tape Gateway, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\. 

If you plan to deploy your gateway in production, you should take your real workload into consideration in determining the disk sizes\. For information on how to determine real\-world disk sizes, see [Managing local disks for your Storage Gateway](ManagingLocalStorage-common.md)\. Also, consider cleaning up if you don't plan to continue using your Tape Gateway\. Cleaning up lets you avoid incurring charges\. For information on cleanup, see [Cleaning Up Resources You Don't Need](#cleanup-vtl)\.

## Cleaning Up Resources You Don't Need<a name="cleanup-vtl"></a>

If you created the gateway as an example exercise or a test, consider cleaning up to avoid incurring unexpected or unnecessary charges\. 

If you plan to continue using your Tape Gateway, see additional information in [Where Do I Go from Here?](#GettingStartedWhatsNextStep3-vtl)

**To clean up resources you don't need**

1. Delete tapes from both your gateway's virtual tape library \(VTL\) and archive\. For more information, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

   1. Archive any tapes that have the **RETRIEVED** status in your gateway's VTL\. For instructions, see [Archiving Tapes](managing-virtual-tapes-vtl.md#main-archiving-tapes-managing-vtl)\.

   1. Delete any remaining tapes from your gateway's VTL\. For instructions, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\.

   1. Delete any tapes you have in the archive\. For instructions, see [Deleting Tapes](managing-gateway-vtl.md#deleting-tapes-vtl)\.

1. Unless you plan to continue using the Tape Gateway, delete it: For instructions, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

1. Delete the Storage Gateway VM from your on\-premises host\. If you created your gateway on an Amazon EC2 instance, terminate the instance\. 