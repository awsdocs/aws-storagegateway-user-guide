# Managing Gateway Updates Using the AWS Storage Gateway Console<a name="MaintenanceManagingUpdate-common"></a>

AWS Storage Gateway periodically releases important software updates for your gateway\. Either you can manually apply updates on the AWS Storage Gateway Management Console, or the updates are automatically applied during the configured maintenance schedule\. Although Storage Gateway checks for updates every week or every month, it only goes through maintenance and restart if there are updates\. 

Before any update is applied to your gateway, AWS notifies you with a message on the Storage Gateway console and your AWS Personal Health Dashboard\. For more information, see [AWS Personal Health Dashboard](https://aws.amazon.com/premiumsupport/phd/)\. The VM doesn't reboot, but the gateway is unavailable for a short period while it's being updated and restarted\.

When you deploy and activate your gateway, a default weekly maintenance schedule is set\. You can modify the maintenance schedule at any time\. When updates are available, the **Details** tab displays a maintenance message\. You can see the date and time that the last successful update was applied to your gateway on the **Details** tab\.

**Important**  
You can minimize the chance of any disruption to your applications due to the gateway restart by increasing the timeouts of your iSCSI initiator\. For more information about increasing iSCSI initiator timeouts for Windows and Linux, see [Customizing Your Windows iSCSI Settings](initiator-connection-common.md#CustomizeWindowsiSCSISettings) and [Customizing Your Linux iSCSI Settings](initiator-connection-common.md#CustomizeLinuxiSCSISettings)\.

**To modify the maintenance schedule**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and choose the gateway that you want to modify the update schedule for\.

1. On the **Actions** menu, choose **Edit maintenance window**\.

1. For **Schedule**, choose **Weekly** to schedule weekly updates\.

1. Modify the values for **Day of the week** and **Time**\.

   Your maintenance start time appears on the **Details** tab for the gateway next time that you open the **Details** tab\.

The update is automatically deployed to your gateway during the configured maintenance schedule\. When the update is being deployed, your gateway becomes unavailable for a short period while the associated services are updated and restarted\.