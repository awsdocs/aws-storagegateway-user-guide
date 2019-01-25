# Managing Gateway Updates Using the AWS Storage Gateway Console<a name="MaintenanceManagingUpdate-common"></a>

AWS Storage Gateway periodically releases important software updates for your gateway\. You can either manually apply updates on the AWS Storage Gateway Management Console or the updates will be automatically applied during the configured weekly maintenance time\. Although Storage Gateway checks for updates every week, it will only go through maintenance and restart if there are updates\. Before any update is applied to your gateway, AWS notifies you with a message on the AWS Storage Gateway Console and your AWS Personal Health Dashboard\. For more information, see [AWS Personal Health Dashboard](https://aws.amazon.com/premiumsupport/phd/)\. The VM will not reboot, but the gateway will be unavailable for a short period while it is being updated and restarted\.

When you deploy and activate your gateway, a default weekly maintenance schedule is set\. You can modify the maintenance schedule at any time\. When updates are available, the **Details** tab displays a maintenance message and an **Apply update now** button\. You can see the date and time that the last successful update was applied to your gateway on the **Details** tab\.

**Important**  
You can minimize the chance of any disruption to your applications due to the gateway restart by increasing the timeouts of your iSCSI initiator\. For more information about increasing iSCSI initiator timeouts for Windows and Linux, see [Customizing Your Windows iSCSI Settings](initiator-connection-common.md#CustomizeWindowsiSCSISettings) and [Customizing Your Linux iSCSI Settings](initiator-connection-common.md#CustomizeLinuxiSCSISettings)\.

**To modify the maintenance schedule**

1. On the navigation menu, choose **Gateways**, and choose the gateway you want to modify the update schedule for\.

1. On the **Actions** menu, choose **Edit maintenance window**\.

1. Modify the values for **Day of the week** and **Time**\. Your changes appear in the **Details** tab for the gateway\.

The update is automatically deployed to your gateway during the configured weekly maintenance time\. When the update is being deployed, your gateway becomes unavailable for a short period while the associated services are updated and restarted\.