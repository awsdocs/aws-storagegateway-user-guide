# Performing Maintenance Tasks on the VMware Local Console<a name="manage-on-premises-vmware"></a>

For a gateway deployed on\-premises, you can perform the following maintenance tasks using the VMware host local console\. 


+ [Accessing the Gateway Local Console with VMware ESXi](#MaintenanceConsoleWindowVMware-common)
+ [Configuring Your Gateway for Multiple NICs in a VMware ESXi Host](#MaintenanceMultiNIC-vmaware)

## Accessing the Gateway Local Console with VMware ESXi<a name="MaintenanceConsoleWindowVMware-common"></a>

**To access your gateway's local console with VMware ESXi**

1. In the VMware vSphere client, select your gateway VM\.

1. Ensure that the gateway is turned on\.
**Note**  
If your gateway VM is turned on, a green arrow icon appears with the VM icon, as shown in the following screenshot\. If your gateway VM is not turned on, you can turn it on by choosing the green **Power On** icon on the **Toolbar** menu\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_65.png)

1. Choose the **Console** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_70.png)

1. After a few moments, the VM is ready for you to log in\.
**Note**  
To release the cursor from the console window, press **Ctrl\+Alt**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)

1. To log in using the default credentials, continue to the procedure [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

## Configuring Your Gateway for Multiple NICs in a VMware ESXi Host<a name="MaintenanceMultiNIC-vmaware"></a>

The following procedure assumes that your gateway VM already has one network adapter defined and that you are adding a second adapter\. The following procedure shows how to add an adapter for VMware ESXi\.

**To configure your gateway to use an additional network adapter in VMware ESXi host**

1. Shut down the gateway\. For instructions, see [To stop a volume or tape gateway](MaintenanceShutDown-common.md#PoweringOffGatewayConsole-common)\.

1. In the VMware vSphere client, select your gateway VM\. 

   The VM can remain turned on for this procedure\.

1. In the client, open the context \(right\-click\) menu for your gateway VM, and choose **Edit Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSProvisionStorageforAppliance_11.png)

1. On the **Hardware** tab of the **Virtual Machine Properties** dialog box, choose **Add** to add a device\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSProvisionStorageforAppliance_20.png)

1. Follow the Add Hardware wizard to add a network adapter\. 

   1. In the **Device Type** pane, choose **Ethernet Adapter** to add an adapter, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayProvisionAdapter_10.png)

   1. In the **Network Type** pane, ensure that **Connect at power on** is selected for **Type**, and then choose **Next**\.

      We recommend that you use the E1000 network adapter with Storage Gateway\. For more information on the adapter types that might appear in the adapter list, see Network Adapter Types in the [ESXi and vCenter Server Documentation](http://pubs.vmware.com/vsphere-50/index.jsp?topic=/com.vmware.vsphere.vm_admin.doc_50/GUID-AF9E24A8-2CFA-447B-AC83-35D563119667.html&resultof=%22VMXNET%22%20%22vmxnet%22)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayProvisionAdapter_15.png)

   1. In the **Ready to Complete** pane, review the information, and then choose **Finish**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayProvisionAdapter_20.png)

1. Choose the **Summary** tab of the VM, and choose **View All** next to the **IP Address** box\. A **Virtual Machine IP Addresses** window displays all the IP addresses you can use to access the gateway\. Confirm that a second IP address is listed for the gateway\.
**Note**  
It might take several moments for the adapter changes to take effect and the VM summary information to refresh\.

   The following image is for illustration only\. In practice, one of the IP addresses will be the address by which the gateway communicates to AWS and the other will be an address in a different subnet\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayProvisionAdapter_25.png)

1. On the Storage Gateway console, turn on the gateway\. For instructions, see [To start a volume or tape gateway](MaintenanceShutDown-common.md#PoweringOnGatewayConsole-common)\.

1. In the **Navigation** pane of the Storage Gateway console, choose **Gateways** and choose the gateway to which you added the adapter\. Confirm that the second IP address is listed in the **Details** tab\.

For information about local console tasks common to VMware and Hyper\-V host, see [Performing Common Maintenance Tasks on the VM Local Console](manage-on-premises-common.md)