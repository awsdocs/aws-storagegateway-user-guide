# Creating a Gateway<a name="create-gateway-vtl"></a>

In this section, you can find instructions about how to download, deploy, and activate a tape gateway\. 

**Topics**
+ [Choosing a Gateway Type](#GettingStartedSelectGatewayType)
+ [Choosing a Host Platform and Downloading the VM](#hosting-options-vtl)
+ [Connecting to Your Gateway](#GettingStartedBeginActivateGateway-vtl)
+ [Activating Your Gateway](#GettingStartedActivateGateway-common)
+ [Configuring Local Disks](#configure-local-disk-alarms)

## Choosing a Gateway Type<a name="GettingStartedSelectGatewayType"></a>

For a [tape gateway](StorageGatewayConcepts.md#storage-gateway-vtl-concepts), you store and archive your data on virtual tapes in AWS\. A tape gateway eliminates some of the challenges associated with owning and operating an on\-premises physical tape infrastructure\.

**To create a tape gateway**

1. Open the AWS Management Console at [http://console\.aws\.amazon\.com/storagegateway/home](http://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the console home page appears\.

1. If you haven't created a gateway in the AWS Region you selected, choose **Get started**\. If you already have a gateway in the AWS Region you selected, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. On the **Select gateway type page**, choose **Tape gateway**, and then choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/tape-gateway.png)

## Choosing a Host Platform and Downloading the VM<a name="hosting-options-vtl"></a>

If you create your gateway on\-premises, you download and deploy the gateway VM and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported Hypervisors and Host Requirements](Requirements.md#requirements-host)\.

**Note**  
You can run only file, cached volume, and tape gateways on an Amazon EC2 instance\.

**To select a host platform and download the VM**

1. On the **Select host platform** page, choose the virtualization platform that you want to run your gateway on\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/HostPlatform.png)

1. Choose **Download image** next to your virtualization platform to download a \.zip file that contains the \.ova file for your virtualization platform\.
**Note**  
The \.zip file is over 500 MB in size and might take some time to download, depending on your network connection\. 

   For EC2, you create an instance from the provided AMI\.

1. Deploy the downloaded image to your hypervisor\. You need to add at least one local disk for your cache and one local disk for your upload buffer during the deployment\. A file gateway requires only one local disk for a cache\. For information about local disk requirements, see [Hardware and Storage Requirements](Requirements.md#requirements-hardware-storage)\.

   If you choose VMware, do the following:
   + Store your disk in **Thick provisioned format**\. When you use thick provisioning, the disk storage is allocated immediately, resulting in better performance\. In contrast, thin provisioning allocates storage on demand\. On\-demand allocation can affect the normal functioning of AWS Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in thick\-provisioned format\.
   + Configure your gateway VM to use paravirtualized disk controllers\. For more information, see [Configuring the AWS Storage Gateway VM to Use Paravirtualized Disk Controllers](configure-vmware.md#SetParaVirtualization-common)\.

   If you choose Microsoft Hyper\-V, do the following: 
   + Configure the disk type as **Fixed size**\. When you use fixed\-size provisioning, the disk storage is allocated immediately, resulting in better performance\. If you don't use fixed\-size provisioning, the storage is allocated on demand\. On\-demand allocation can affect the functioning of AWS Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in fixed\-size provisioned format\. 
   + When allocating disks, choose **virtual hard disk \(\.vhd\) file**\. Storage Gateway supports the \.vhdx file type\. By using this file type, you can create larger virtual disks than with other file types\. If you create a \.vhdx type virtual disk, make sure that the size of the virtual disks that you create doesn't exceed the recommended disk size for your gateway\.

   For both VMware and Microsoft Hyper\-V, synchronizing the VM time with the host time is required for successful gateway activation\. Make sure that your host clock is set to the correct time and synchronize it with a Network Time Protocol \(NTP\) server\. 

For information about deploying your gateway to an Amazon EC2 host, see [Deploy your gateway to an Amazon EC2 host](ec2-gateway-common.md)\.

## Connecting to Your Gateway<a name="GettingStartedBeginActivateGateway-vtl"></a>

To connect to your gateway, the first step is to get the IP address of your gateway VM\. You use this IP address to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address from the Amazon EC2 console\.

The activation process associates your gateway with your AWS account\. Your gateway VM must be running for activation to succeed\.

Make sure that you connect to the correct gateway type\. The \.ova files and AMIs for the gateway types are different and are not interchangeable\.

**To get the IP address for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi—[Accessing the Gateway Local Console with VMware ESXi](manage-on-premises-vmware.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V—[Access the Gateway Local Console with Microsoft Hyper\-V](manage-on-premises-Hyperv.md#MaintenanceConsoleWindowHyperV-common)\.

1. Get the IP address from the top of the menu page, and make note of it for later use\.

**To get the IP address from an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose the EC2 instance\.

1. Choose the **Description** tab at the bottom, and then note the IP address\. You use this IP address to activate the gateway\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/step7c-get-ip-address.png)

For activation, you can use the public or private IP address assigned to a gateway\. You must be able to reach the IP address that you use from the browser from which you perform the activation\. In this walkthrough, we use the public IP address to activate the gateway\.

**To associate your gateway with your AWS account**

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Type the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BeginActivation.png)

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.

## Activating Your Gateway<a name="GettingStartedActivateGateway-common"></a>

When your gateway VM is deployed and running, you can configure your gateway settings and activate your gateway\. If activation fails, check that the IP address you entered is correct\. If the IP address is correct, confirm that your network is configured to let your browser access the gateway VM\. For more information on troubleshooting, see [Troubleshooting On\-Premises Gateway Issues](GatewayTroubleshooting.md) or [Troubleshooting Amazon EC2 Gateway Issues](EC2GatewayTroubleshooting.md)\.

**To configure your gateway settings**

1. Type the information listed on the activation page to configure your gateway settings and complete the activation process\.

   The following screenshot shows the activation page for tape gateways\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-gateway-vtl.png)
   + **AWS Region** specifies the AWS Region where your gateway will be created and your data stored\.
   + **Gateway time zone** specifies the time zone to use for your gateway\.
   + **Gateway name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\. 
   + **Backup application** specifies the backup application you want to use\. Storage Gateway automatically chooses a compatible medium changer for your backup application\. If your backup application is not listed, choose **Other** and choose a medium changer type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-vtl-other.png)

      **Medium changer type** specifies the type of medium changer to use for your backup application\.

     The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with tape gateways\. This table includes the medium changer type recommended for each backup application\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/create-gateway-vtl.html)
**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](resource_vtl-devices.md#change-mediumchanger-vtl)\.
   + **Tape drive type** specifies the type of tape drive used by this gateway\. 

1. Choose **Activate gateway**\.

   When the gateway is successfully activated, the AWS Storage Gateway console displays the **Configure local storage** page\.

   If activation is not successful, see [Troubleshooting Your Gateway](Troubleshooting-common.md) for possible solutions\.

## Configuring Local Disks<a name="configure-local-disk-alarms"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\.

**Note**  
If you allocate local disks on a VMware host, make sure to configure the disks to use paravirtualized disk controllers\.  
When adding a cache or upload buffer to an existing gateway, make sure to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.<a name="local-storage-cached"></a>

**To configure local disks**

1. On the **Configure local disks** page, identify the disks you allocated and decide which ones you want to use for an upload buffer and cached storage\. For information about disk size limits, see [Configuration and Performance Limits](resource-gateway-limits.md#performance-limits)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-local-storage.png)

1. In the **Allocation** column next to your upload buffer disk, choose **Upload Buffer**\.

1. Choose **Cache** for the disk you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

**Next Step**

[Creating Tapes](GettingStartedCreateTapes.md)