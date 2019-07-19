# Creating a Gateway<a name="create-gateway-file"></a>

In this section, you can find instructions about how to download, deploy, and activate your file gateway\. 

**Topics**
+ [Choosing a Gateway Type](#GettingStartedSelectGatewayType-file)
+ [Choosing a Host Platform and Downloading the VM](#hosting-options-file)
+ [Choosing a Service Endpoint](#GettingStarted-service-endpoint-file)
+ [Connecting to Your Gateway](#GettingStartedBeginActivateGateway-file)
+ [Activating Your Gateway](#GettingStartedActivateGateway-file)
+ [Configuring Local Disks](#configure-local-storage-alarms-file)

## Choosing a Gateway Type<a name="GettingStartedSelectGatewayType-file"></a>

With a file gateway, you store and retrieve objects in Amazon S3 with a local cache for low latency access to your most recently used data\. 

**To choose a gateway type**

1. Open the AWS Management Console at [http://console\.aws\.amazon\.com/storagegateway/home](http://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the service homepage appears\.

1. If you haven't created a gateway in the AWS Region that you chose, choose **Get started**\. If you already have a gateway in the AWS Region that you chose, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. On the **Select gateway type** page, choose **File gateway**, and then choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/file-gateway.png)

## Choosing a Host Platform and Downloading the VM<a name="hosting-options-file"></a>

If you create your gateway on\-premises, you deploy the hardware appliance, or download and deploy a gateway VM, and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported Hypervisors and Host Requirements](Requirements.md#requirements-host)\.

**Note**  
You can run only file, cached volume, and tape gateways on an Amazon EC2 instance\.

**To select a host platform and download the VM**

1. On the **Select host platform** page, choose the virtualization platform that you want to run your gateway on\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSelectHostPlatform.png)  
  


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

   If you choose EC2, do the following:

   Launch an Amazon Machine Image \(AMI\) that contains the gateway VM image, and then activate the gateway\. For information about deploying your gateway to an Amazon EC2 host, see: [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md) 

   If you choose the hardware appliance, see [Activate Your Hardware Appliance](appliance-activation.md)\.

For information about deploying your gateway to an Amazon EC2 host, see [Deploy Your Gateway to an Amazon EC2 Host](ec2-gateway-file.md)\.

## Choosing a Service Endpoint<a name="GettingStarted-service-endpoint-file"></a>

You can activate your gateway using a public endpoint and have your gateway communicate with AWS storage services over the public Internet or activate it using a private VPC endpoint\. If you use a VPC endpoint, all communication from your gateway to AWS services occurs through the VPC endpoint in your VPC in AWS\. 

**To choose a service endpoint**

1. For **Endpoint type** you have the following options:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/choose-endpoint-public.png)

   To make your gateway access AWS services over the public Internet, choose **Public**\. 

   To make your gateway access AWS services through the VPC endpoint in your VPC, choose **VPC**\. 

   This walkthorough assumes that you are activating your gateway with a public endpoint\. For Information about how to activate a gateway using a VPC, endpoint see [Activating a Gateway in a Virtual Private Cloud](gateway-private-link.md)\.

1. Choose **Next** to connect you gateway and activate your gateway\.

## Connecting to Your Gateway<a name="GettingStartedBeginActivateGateway-file"></a>

To connect to your gateway, the first step is to get the IP address of your gateway VM\. You use this IP address to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address from the Amazon EC2 console\.

The activation process associates your gateway with your AWS account\. Your gateway VM must be running for activation to succeed\.

Make sure that you select the correct gateway type\. The \.ova files and AMIs for the gateway types are different and are not interchangeable\.

**To get the IP address for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi—[Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V—[Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.

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

## Activating Your Gateway<a name="GettingStartedActivateGateway-file"></a>

**To activate your gateway**

The gateway type, endpoint type, and AWS Region you selected are shown on the activation page\.

1. To complete the activation process, provide information on the activation page to configure your gateway setting:
   + **Gateway Time Zone** specifies the time zone to use for your gateway\.
   + **Gateway Name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\. 

     The following screenshot shows the activation page for a file gateway\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-gateway.png)

1. **AWS Region** specifies the AWS Region where your gateway will be activated and where your data will be stored\. If **Endpoint type** is **VPC**, the AWS Region should be same as the Region where your VPC Endpoint is located\.

1. Choose **Activate gateway**\.

1. If activation is not successful, see [Troubleshooting Your Gateway](Troubleshooting-common.md) for possible solutions\.

## Configuring Local Disks<a name="configure-local-storage-alarms-file"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\. <a name="local-storage-cached-file"></a>

**To configure local disks**

1. On the **Configure local disks** page, identify the disks you added and decide which ones you want to allocate for cached storage\. For information about disk size limits, see [Recommended Local Disk Sizes For Your Gateway](resource-gateway-limits.md#disk-sizes)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-local-storage-file.png)

1. Choose **Cache** for the disk you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

**Next Step**

[Creating a File Share](GettingStartedCreateFileShare.md)