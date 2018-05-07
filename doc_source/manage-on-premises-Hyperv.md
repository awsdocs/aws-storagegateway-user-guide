# Performing Maintenance Tasks on the Hyper V Local Console<a name="manage-on-premises-Hyperv"></a>

For a gateway deployed on\-premises, you can perform the following maintenance tasks using the Hyper V host local console\. 

**Topics**
+ [Access the Gateway Local Console with Microsoft Hyper\-V](#MaintenanceConsoleWindowHyperV-common)
+ [Synchronizing Your Gateway VM Time](#MaintenanceTimeSync-hyperv)
+ [Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host](#MaintenanceMultiNIC-hyperv)

## Access the Gateway Local Console with Microsoft Hyper\-V<a name="MaintenanceConsoleWindowHyperV-common"></a>

**To access your gateway's local console \(Microsoft Hyper\-V\)**

1. In the **Virtual Machines** list of the Microsoft Hyper\-V Manager, select your gateway VM\.

1. Ensure the gateway is turned on\.
**Note**  
If your gateway VM is turned on, `Running` is displayed as the **State** of the VM, as shown in the following screenshot\. If your gateway VM is not turned on, you can turn it on by choosing **Start** in the **Actions** pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-manager09.png)

1. In the **Actions** pane, choose **Connect**\.

   The **Virtual Machine Connection** window appears\. If an authentication window appears, type the user name and password provided to you by the hypervisor administrator\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-vm-connect01.png)

1.  After a few moments, the VM is ready for you to log in\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)

1. To log in default credentials, continue to the procedure [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

## Synchronizing Your Gateway VM Time<a name="MaintenanceTimeSync-hyperv"></a>

For a gateway deployed on VMware ESXi, setting the hypervisor host time and synchronizing the VM time to the host is sufficient to avoid time drift\. For more information, see [Synchronizing VM Time with Host Time](configure-vmware.md#GettingStartedSyncVMTime-common)\. For a gateway deployed on Microsoft Hyper\-V, you should periodically check your VM's time using the procedure described following\.

**To view and synchronize the time of a Hyper\-V gateway VM to an NTP server**

1. Log in to your gateway's local console\.
   + VMware ESXi—for more information, see [Accessing the Gateway Local Console with VMware ESXi](manage-on-premises-vmware.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V—for more information, see [Access the Gateway Local Console with Microsoft Hyper\-V](#MaintenanceConsoleWindowHyperV-common)\.

1. On the **AWS Storage Gateway Configuration** main menu, type **4** for **System Time Management**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/LocalConsoleLogin.png)

1. On the **System Time Management** menu, type **1** for **View and Synchronize System Time**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync01.png)

1. If the result indicates that you should synchronize your VM's time to the Network Time Protocol \(NTP\) time, type **y**\. Otherwise, type **n**\.

   If you type **y** to synchronize, the synchronization might take a few moments\.

   The following screenshot shows a VM that does not require time synchronization\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync02.png)

   The following screenshot shows a VM that does require time synchronization\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync03.png)

## Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host<a name="MaintenanceMultiNIC-hyperv"></a>

The following procedure assumes that your gateway VM already has one network adapter defined and that you are adding a second adapter\. This procedure shows how to add an adapter for a Microsoft Hyper\-V host\.

**To configure your gateway to use an additional network adapter in a Microsoft Hyper\-V Host**

1. On the Storage Gateway console, turn off the gateway\. For instructions, see [To stop a volume or tape gateway](MaintenanceShutDown-common.md#PoweringOffGatewayConsole-common)\.

1. In the Microsoft Hyper\-V Manager, select your gateway VM\.

1. If the VM isn't turned off already, open the context \(right\-click\) menu for your gateway and choose **Turn Off**\.

1. In the client, open the context menu for your gateway VM and choose **Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-manager10.png)

1. In the **Settings** dialog box for the VM, for **Hardware**, choose **Add Hardware**\.

1. In the **Add Hardware** pane, choose **Network Adapter**, and then choose **Add** to add a device\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-vm-settings15.png)

1. Configure the network adapter, and then choose **Apply** to apply settings\.

   In the following example, **Virtual Network 2** is selected for the new adapter\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-vm-settings16.png)

1. In the **Settings** dialog box, for **Hardware**, confirm that the second adapter was added, and then choose **OK**\.

1. On the Storage Gateway console, turn on the gateway\. For instructions, see [To start a volume or tape gateway](MaintenanceShutDown-common.md#PoweringOnGatewayConsole-common)\.

1. In the **Navigation** pane choose **Gateways**, then select the gateway to which you added the adapter\. Confirm that the second IP address is listed in the **Details** tab\.

For information about local console tasks common to VMware and Hyper\-V host, see [Performing Common Maintenance Tasks on the VM Local Console](manage-on-premises-common.md)