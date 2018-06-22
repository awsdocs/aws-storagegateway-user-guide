# Where Do I Go from Here?<a name="GettingStartedWhatsNextStep3File"></a>

In the preceding sections, you created and started using a file gateway, including mounting a file share and testing your setup\. 

Other sections of this guide include information about how to do the following:
+ To manage your file gateway, see [Managing Your File Gateway](managing-gateway-file.md)\.
+ To optimize your file gateway, see [Optimizing Gateway Performance](Optimizing-common.md)\.
+ To troubleshoot gateway problems, see [Troubleshooting Your Gateway](Troubleshooting-common.md)\.
+ To learn about Storage Gateway metrics and how you can monitor how your gateway performs, see [Monitoring Your Gateway and Resources](Main_monitoring-gateways-common.md)\.

## Cleaning Up Resources You Don't Need<a name="cleanup-file"></a>

If you created your gateway as an example exercise or a test, consider cleaning up to avoid incurring unexpected or unnecessary charges\. 

**To clean up resources you don't need**

1. Delete any snapshots\. For instructions, see [Deleting a Snapshot](managing-volumes.md#DeletingASnapshot)\.

1. Unless you plan to continue using the gateway, delete it\. For more information, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

1. Delete the AWS Storage Gateway VM from your on\-premises host\. If you created your gateway on an Amazon EC2 instance, terminate the instance\. 