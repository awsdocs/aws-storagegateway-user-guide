# Managing Gateway Updates Using the Storage Gateway Console<a name="MaintenanceManagingUpdate-common"></a>

Storage Gateway periodically releases important software updates for your gateway\. Either you can manually apply updates on the Storage Gateway Management Console, or the updates are automatically applied during the configured maintenance schedule\. Although Storage Gateway checks for updates every minute, it only goes through maintenance and restarts if there are updates\. 

Before any update is applied to your gateway, AWS notifies you with a message on the Storage Gateway console and your AWS Personal Health Dashboard\. For more information, see [AWS Personal Health Dashboard](http://aws.amazon.com/premiumsupport/phd/)\. The VM doesn't reboot, but the gateway is unavailable for a short period while it's being updated and restarted\.

When you deploy and activate your gateway, a default weekly maintenance schedule is set\. You can modify the maintenance schedule at any time\. When updates are available, the **Details** tab displays a maintenance message\. You can see the date and time that the last successful update was applied to your gateway on the **Details** tab\.

**To modify the maintenance schedule**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and choose the gateway that you want to modify the update schedule for\.

1. For **Actions**, choose **Edit maintenance window** to pen the Edit maintenance start time dialog box\.

1. For **Schedule**, choose **Weekly** or **Monthly** to schedule updates\.

1. If you choose **Weekly**, modify the values for **Day of the week** and **Time**\.

   If you choose **Monthly**, modify the values for **Day of the month** and **Time**\. If you choose this option and you get an error, it means your gateway is an older version and has not been upgraded to a newer version yet\.
**Note**  
The maximum value that can be set for day of the month is 28\. If 28 is selected, the maintenance start time will be on the 28th day of every month\.

   Your maintenance start time appears on the **Details** tab for the gateway next time that you open the **Details** tab\.