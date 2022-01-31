--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Accessing the Gateway Local Console<a name="accessing-local-console"></a>

How you access your VM's local console depends on the type of the Hypervisor you deployed your gateway VM on\. In this section, you can find information on how to access the VM local console using Linux Kernel\-based Virtual Machine \(KVM\), VMware ESXi, and Microsoft Hyper\-V Manager\.

For instructions to access the gateway local console for Tape Gateway on Snowball Edge, see [Troubleshooting and best practices for Tape Gateway on Snowball Edge](https://docs.aws.amazon.com/storagegateway/latest/userguide/using-tape-gateway-snowball.html#troubleshooting-best-practices-tape-gateway-snowball)\.

**Topics**
+ [Accessing the Gateway Local Console with Linux KVM](#MaintenanceConsoleWindowKVM-common)
+ [Accessing the Gateway Local Console with VMware ESXi](#MaintenanceConsoleWindowVMware-common)
+ [Access the Gateway Local Console with Microsoft Hyper\-V](#MaintenanceConsoleWindowHyperV-common)

## Accessing the Gateway Local Console with Linux KVM<a name="MaintenanceConsoleWindowKVM-common"></a>

There are different ways to configure virtual machines running on KVM, depending on the Linux distribution being used\. Instructions for accessing KVM configuration options from the command line follow\. Instructions might differ depending on your KVM implementation\.

**To access your gateway's local console with KVM**

1. Use the following command to list the VMs that are currently available in KVM\. 

   ```
   # virsh list
   ```

   You can choose available VMs by `Id`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_52.png)

1. Use the following command to access the local console\.

   ```
   # virsh console VM_Id
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_51.png)

1. To get default credentials to log in to the local console, see [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

1. After you have logged in, you can activate and configure your gateway\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_50.png)

## Accessing the Gateway Local Console with VMware ESXi<a name="MaintenanceConsoleWindowVMware-common"></a>



**To access your gateway's local console with VMware ESXi**

1. In the VMware vSphere client, select your gateway VM\.

1. Make sure that the gateway is turned on\.
**Note**  
If your gateway VM is turned on, a green arrow icon appears with the VM icon, as shown in the following screenshot\. If your gateway VM is not turned on, you can turn it on by choosing the green **Power On** icon on the **Toolbar** menu\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_65.png)

1. Choose the **Console** tab\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_70.png)

   After a few moments, the VM is ready for you to log in\.
**Note**  
To release the cursor from the console window, press **Ctrl\+Alt**\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)

1. To log in using the default credentials, continue to the procedure [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

## Access the Gateway Local Console with Microsoft Hyper\-V<a name="MaintenanceConsoleWindowHyperV-common"></a>



**To access your gateway's local console \(Microsoft Hyper\-V\)**

1. In the **Virtual Machines** list of the Microsoft Hyper\-V Manager, select your gateway VM\.

1. Make sure that the gateway is turned on\.

    
**Note**  
If your gateway VM is turned on, `Running` is displayed as the **State** of the VM, as shown in the following screenshot\. If your gateway VM is not turned on, you can turn it on by choosing **Start** in the **Actions** pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-manager09.png)

1. In the **Actions** pane, choose **Connect**\.

   The **Virtual Machine Connection** window appears\. If an authentication window appears, type the user name and password provided to you by the hypervisor administrator\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-vm-connect01.png)

    After a few moments, the VM is ready for you to log in\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)

1. To log in using the default credentials, continue to the procedure [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.