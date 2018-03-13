# Shutting Down Your Gateway VM<a name="MaintenanceShutDown-common"></a>

You might need to shutdown or reboot your VM for maintenance, such as when applying a patch to your hypervisor\. Before you shutdown the VM, you must first stop the gateway\. For file gateway, you just shutdown your VM\. Although this section focuses on starting and stopping your gateway using the AWS Storage Gateway Management Console, you can also and stop your gateway by using your VM local console or AWS Storage Gateway API\. When you power on your VM, remember to restart your gateway\. 

+ Gateway VM local console—see [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

+ AWS Storage Gateway API\-—see [ShutdownGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ShutdownGateway.html) 

**Note**  
If you stop your gateway while your backup software is writing or reading from a tape, the write or read task might not succeed\. Before you stop your gateway, you should check your backup software and the backup schedule for any tasks in progress\.

For file gateway, you simply shutdown your VM\. You don't shutdown the gateway\.

## Starting and Stopping a Volume or Tape Gateway<a name="start-stop-classic"></a>

The following instructions apply to volume and tape gateways only\. <a name="PoweringOffGatewayConsole-common"></a>

**To stop a volume or tape gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway to stop\. The status of the gateway is **Running**\.

1. On the **Actions** menu, choose **Stop gateway** and verify the id of the gateway from the dialog box, and then choose **Stop gateway**\.

   While the gateway is stopping, you might see a message that indicates the status of the gateway\. When the gateway shuts down, a message and a **Start gateway** button appears in the **Details** tab\.

When you stop your gateway, the storage resources will not be accessible until you start your storage\. If the gateway was uploading data when it was stopped, the upload will resume when you start the gateway\.<a name="PoweringOnGatewayConsole-common"></a>

**To start a volume or tape gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways** and then choose the gateway to start\. The status of the gateway is **Shutdown**\.

1. Choose **Details**\. and then choose **Start gateway**\.