# Configuring Network Adapters for Your Gateway<a name="configure-multi-nic"></a>

In this section you can find information about how configure multiple network adapters for your gateway\.

**Topics**
+ [Configuring Your Gateway for Multiple NICs in a VMware ESXi Host](#MaintenanceMultiNIC-vmaware)
+ [Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host](#MaintenanceMultiNIC-hyperv)

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

For information about local console tasks common to VMware and Hyper\-V host, see [Performing Tasks on the VM Local Console \(Volume and Tape Gateways\)](manage-on-premises-common.md)

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

For information about local console tasks common to VMware and Hyper\-V host, see [Performing Tasks on the VM Local Console \(Volume and Tape Gateways\)](manage-on-premises-common.md)