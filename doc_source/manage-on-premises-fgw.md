--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Performing tasks on the VM local console \(file gateway\)<a name="manage-on-premises-fgw"></a>

For a file gateway deployed on\-premises, you can perform the following maintenance tasks using the VM host's local console\. These tasks are common to VMware, Microsoft Hyper\-V, and Linux Kernel\-based Virtual Machine \(KVM\) hypervisors\.

**Topics**
+ [Logging in to the file gateway local console](#LocalConsole-login-fgw)
+ [Configuring an HTTP proxy](#MaintenanceRoutingProxy-fgw)
+ [Configuring your gateway network settings](#MaintenanceConfiguringStaticIP-fgw)
+ [Testing your gateway's network connectivity](#MaintenanceTestGatewayConnectivity-fgw)
+ [Viewing your gateway system resource status](#system-resource-check-fgw)
+ [Configuring a Network Time Protocol \(NTP\) server for your gateway](#MaintenanceTimeSync-fgw)
+ [Running storage gateway commands on the local console](#MaintenanceGatewayConsole-fgw)
+ [Configuring network adapters for your gateway](#NICConfiguring-fgw)

## Logging in to the file gateway local console<a name="LocalConsole-login-fgw"></a>

When the VM is ready for you to log in, the login screen is displayed\. If this is your first time logging in to the local console, you use the default user name and password to log in\. These default login credentials give you access to menus where you can configure gateway network settings and change the password from the local console\. AWS Storage Gateway enables you to set your own password from the Storage Gateway console instead of changing the password from the local console\. You don't need to know the default password to set a new password\. For more information, see [Setting the Local Console Password from the Storage Gateway Console](manage-on-premises-common.md#set-password)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)<a name="MaintenanceConsoleWindowMenu-fgw"></a>

**To log in to the gateway's local console**
+ If this is your first time logging in to the local console, log in to the VM with the default credentials\. The default user name is `admin` and the password is `password`\. Otherwise, use your credentials to log in\.
**Note**  
We recommend changing the default password\. You do this by running the `passwd` command from the local console menu \(item 6 on the main menu\)\. For information about how to run the command, see [Running storage gateway commands on the local console](#MaintenanceGatewayConsole-fgw)\. You can also set the password from the Storage Gateway console\. For more information, see [Setting the Local Console Password from the Storage Gateway Console](manage-on-premises-common.md#set-password)\.

### Setting the local console password from the Storage Gateway console<a name="set-password-fgw"></a>

When you log in to the local console for the first time, you log in to the VM with the default credentials\. For all types of gateways, you use default credentials\. The user name is `admin` and the password is `password`\.

We recommend that you always set a new password immediately after you create your new gateway\. You can set this password from the AWS Storage Gateway console rather than the local console if you want\. You don't need to know the default password to set a new password\.

**To set the local console password on the Storage Gateway console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and then choose the gateway for which you want to set a new password\.

1. For **Actions**, choose **Set Local Console Password**\.

1. In the **Set Local Console Password** dialog box, enter a new password, confirm the password, and then choose **Save**\. 

   Your new password replaces the default password\. Storage Gateway doesn't save the password but rather safely transmits it to the VM\.
**Note**  
The password can consist of any character on the keyboard and can be 1–512 characters long\.

## Configuring an HTTP proxy<a name="MaintenanceRoutingProxy-fgw"></a>

 File gateways support configuration of an HTTP proxy\. 

**Note**  
The only proxy configuration that file gateways support is HTTP\.

If your gateway must use a proxy server to communicate to the internet, then you need to configure the HTTP proxy settings for your gateway\. You do this by specifying an IP address and port number for the host running your proxy\. After you do so, Storage Gateway routes all AWS endpoint traffic through your proxy server\. Communications between the gateway and endpoints is encrypted, even when using the HTTP proxy\. For information about network requirements for your gateway, see [Network and firewall requirements](Requirements.md#networks)\.<a name="http-proxy-fgw"></a>

**To configure an HTTP proxy for a file gateway**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the local console for the Linux Kernel\-Based Virtual Machine \(KVM\), see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. On the **AWS Appliance Activation \- Configuration** main menu, enter **1** to begin configuring the HTTP proxy\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

1. On the **HTTP Proxy Configuration menu**, enter **1** and provide the host name for the HTTP proxy server\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-1.png)

   You can configure other HTTP settings from this menu as shown following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-fgw.html)

1. Restart your VM to apply your HTTP configuration settings\.

## Configuring your gateway network settings<a name="MaintenanceConfiguringStaticIP-fgw"></a>

The default network configuration for the gateway is Dynamic Host Configuration Protocol \(DHCP\)\. With DHCP, your gateway is automatically assigned an IP address\. In some cases, you might need to manually assign your gateway's IP as a static IP address, as described following\.

**To configure your gateway to use static IP addresses**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. On the **AWS Appliance Activation \- Configuration** main menu, enter **2** to begin configuring your network\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

1. On the ** Network Configuration** menu, choose one of the following options\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-2.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-fgw.html)

## Testing your gateway's network connectivity<a name="MaintenanceTestGatewayConnectivity-fgw"></a>

You can use your gateway's local console to test your network connectivity\. This test can be useful when you are troubleshooting network issues with your gateway\.

**To test your gateway's network connectivity**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **Test Network Connectivity**\.

   If your gateway has already been activated, the connectivity test begins immediately\. For gateways that have not yet been activated, you must specify the endpoint type and AWS Region as described in the following steps\.

1. If your gateway is not yet activated, enter the corresponding numeral to select the endpoint type for your gateway\.

1. If you selected the public endpoint type, enter the corresponding numeral to select the AWS Region that you want to test\. For supported AWS Regions and a list of AWS service endpoints you can use with Storage Gateway, see [AWS Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

As the test progresses, each endpoint displays either **\[PASSED\]** or **\[FAILED\]**, indicating the status of the connection as follows:


| Message | Description | 
| --- | --- | 
| \[PASSED\] | Storage Gateway has network connectivity\.  | 
| \[FAILED\] | Storage Gateway does not have network connectivity\.  | 

## Viewing your gateway system resource status<a name="system-resource-check-fgw"></a>

When your gateway starts, it checks its virtual CPU cores, root volume size, and RAM\. It then determines whether these system resources are sufficient for your gateway to function properly\. You can view the results of this check on the gateway's local console\.

**To view the status of a system resource check**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. In the **AWS Appliance Activation \- Configuration** main menu, enter **4** to view the results of a system resource check\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

   The console displays an **\[OK**\], **\[WARNING\]**, or **\[FAIL\]** message for each resource as described in the table following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-fgw.html)

   The console also displays the number of errors and warnings next to the resource check menu option\.

## Configuring a Network Time Protocol \(NTP\) server for your gateway<a name="MaintenanceTimeSync-fgw"></a>

You can view and edit Network Time Protocol \(NTP\) server configurations and synchronize the VM time on your gateway with your hypervisor host\.

**To manage system time**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. In the **AWS Appliance Activation \- Configuration** main menu, enter **5** to manage your system's time\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

1. In the **System Time Management** menu, choose one of the following options\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-4.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-fgw.html)

## Running storage gateway commands on the local console<a name="MaintenanceGatewayConsole-fgw"></a>

The VM local console in Storage Gateway helps provide a secure environment for configuring and diagnosing issues with your gateway\. Using the local console commands, you can perform maintenance tasks such as saving routing tables, connecting to AWS Support, and so on\.

**To run a configuration or diagnostic command**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. On the **AWS Appliance Activation \- Configuration** main menu, enter **6** for **Command Prompt**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

1. On the **AWS Appliance Activation \- Command Prompt** console, enter **h**, and then press the **Return** key\.

   The console displays the **AVAILABLE COMMANDS** menu with what the commands do, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/available-commands.png)

1. At the command prompt, enter the command that you want to use and follow the instructions\.

To learn about a command, enter the command name at the command prompt\.

## Configuring network adapters for your gateway<a name="NICConfiguring-fgw"></a>

By default, Storage Gateway is configured to use the E1000 network adapter type, but you can reconfigure your gateway to use the VMXNET3 \(10 GbE\) network adapter\. You can also configure Storage Gateway so it can be accessed by more than one IP address\. You do this by configuring your gateway to use more than one network adapter\.

**Topics**
+ [Configuring your gateway to use the VMXNET3 network adapter](#NICChanging-fgw)
+ [Configuring your gateway for multiple NICs](#MaintenanceMultiNIC-fgw)

### Configuring your gateway to use the VMXNET3 network adapter<a name="NICChanging-fgw"></a>

Storage Gateway supports the E1000 network adapter type in both VMware ESXi and Microsoft Hyper\-V hypervisor hosts\. However, the VMXNET3 \(10 GbE\) network adapter type is supported in VMware ESXi hypervisor only\. If your gateway is hosted on a VMware ESXi hypervisor, you can reconfigure your gateway to use the VMXNET3 \(10 GbE\) adapter enter\. For more information on this adapter, see the [VMware website](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001805)\.

For KVM hypervisor hosts, Storage Gateway supports the use of `virtio` network device drivers\. Use of the E1000 network adapter type for KVM hosts isn't supported\.

**Important**  
To select VMXNET3, your guest operating system enter must be **Other Linux64**\.

Following are the steps you take to configure your gateway to use the VMXNET3 adapter:

1. Remove the default E1000 adapter\.

1. Add the VMXNET3 adapter\.

1. Restart your gateway\.

1. Configure the adapter for the network\.

Details on how to perform each step follow\.

**To remove the default E1000 adapter and configure your gateway to use the VMXNET3 adapter**

1. In VMware, open the context \(right\-click\) menu for your gateway and choose **Edit Settings**\.

1. In the **Virtual Machine Properties** window, choose the **Hardware** tab\.

1. For **Hardware**, choose **Network adapter**\. Notice that the current adapter is E1000 in the **Adapter Enter** section\. You replace this adapter with the VMXNET3 adapter\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/VerifyAdapterType.png)

1. Choose the E1000 network adapter, and then choose **Remove**\. In this example, the E1000 network adapter is **Network adapter 1**\.
**Note**  
Although you can run the E1000 and VMXNET3 network adapters in your gateway at the same time, we don't recommend doing so because it can cause network problems\.

1. Choose **Add** to open the Add Hardware wizard\.

1. Choose **Ethernet Adapter**, and then choose **Next**\.

1. In the Network Enter wizard, select **VMXNET3** for **Adapter Enter**, and then choose **Next**\.

1. In the Virtual Machine properties wizard, verify in the **Adapter Enter** section that **Current Adapter** is set to **VMXNET3**, and then choose **OK**\.

1. In the VMware VSphere client, shut down your gateway\.

1. In the VMware VSphere client, restart your gateway\.

After your gateway restarts, reconfigure the adapter you just added to make sure that network connectivity to the internet is established\.

**To configure the adapter for the network**

1. In the VSphere client, choose the **Console** tab to start the local console\. Use the default login credentials to log in to the gateway's local console for this configuration task\. For information about how to log in using the default credentials, see [Logging in to the file gateway local console](#LocalConsole-login-fgw)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_75.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/local-console-file-0.png)

1. At the prompt, enter **2** to select **Network Configuration**, and then press **Enter** to open the network configuration menu\.

1. At the prompt, enter **4** to select **Reset all to DHCP**, and then enter **y** \(for yes\) at the prompt to set all adapters to use Dynamic Host Configuration Protocol \(DHCP\)\. All available adapters are set to use DHCP\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ResetDHCP.png)

   If your gateway is already activated, you must shut it down and restart it from the Storage Gateway Management Console\. After the gateway restarts, you must test network connectivity to the internet\. For information about how to test network connectivity, see [Testing your gateway's network connectivity](#MaintenanceTestGatewayConnectivity-fgw)\.

### Configuring your gateway for multiple NICs<a name="MaintenanceMultiNIC-fgw"></a>

If you configure your gateway to use multiple network adapters \(NICs\), it can be accessed by more than one IP address\. You might want to do this in the following situations:
+ ****Maximizing throughput**** – You might want to maximize throughput to a gateway when network adapters are a bottleneck\.
+ ****Application separation**** – You might need to separate how your applications write to a gateway's volumes\. For example, you might choose to have a critical storage application exclusively use one particular adapter defined for your gateway\.
+ ****Network constraints**** – Your application environment might require that you keep your iSCSI targets and the initiators that connect to them in an isolated network\. This network is different from the network by which the gateway communicates with AWS\.

In a typical multiple\-adapter use case, one adapter is configured as the route by which the gateway communicates with AWS \(that is, as the default gateway\)\. Except for this one adapter, initiators must be in the same subnet as the adapter that contains the iSCSI targets to which they connect\. Otherwise, communication with the intended targets might not be possible\. If a target is configured on the same adapter that is used for communication with AWS, then iSCSI traffic for that target and AWS traffic flows through the same adapter\.

In some cases, you might configure one adapter to connect to the Storage Gateway console and then add a second adapter\. In such a case, Storage Gateway automatically configures the route table to use the second adapter as the preferred route\. For instructions on how to configure multiple adapters, see the following sections:
+ [Configuring Your Gateway for Multiple NICs in a VMware ESXi Host](configure-multi-nic.md#MaintenanceMultiNIC-vmaware)
+ [Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host](configure-multi-nic.md#MaintenanceMultiNIC-hyperv)