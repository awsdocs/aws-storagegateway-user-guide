# Creating a Gateway<a name="create-gateway-vtl"></a>

In this section, you can find instructions about how to download, deploy, and activate a tape gateway\. 

**Topics**
+ [Choosing a Gateway Type](#GettingStartedSelectGatewayType)
+ [Choosing a Host Platform and Downloading the VM](#hosting-options-vtl)
+ [Choosing a Service Endpoint](#GettingStarted-service-endpoint-tape)
+ [Connecting to Your Gateway](#GettingStartedBeginActivateGateway-vtl)
+ [Activating Your Gateway](#GettingStartedActivateGateway-common)
+ [Configuring Local Disks](#configure-local-disk-alarms)
+ [Configuring Amazon CloudWatch Logging](#configure-logging-tape)
+ [Verify VMware High Availability \(VMware HA Clusters Only\)](#verify-ha-tape)

## Choosing a Gateway Type<a name="GettingStartedSelectGatewayType"></a>

For a [tape gateway](StorageGatewayConcepts.md#storage-gateway-vtl-concepts), you store and archive your data on virtual tapes in AWS\. A tape gateway eliminates some of the challenges associated with owning and operating an on\-premises physical tape infrastructure\.

**To create a tape gateway**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the console home page appears\.

1. If you haven't created a gateway in the AWS Region you selected, choose **Get started**\. If you already have a gateway in the AWS Region you selected, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. On the **Select gateway type page**, choose **Tape gateway**, and then choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/tape-gateway.png)

## Choosing a Host Platform and Downloading the VM<a name="hosting-options-vtl"></a>

If you create your gateway on\-premises, you deploy the hardware appliance, or download and deploy a gateway VM, and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported hypervisors and host requirements](Requirements.md#requirements-host)\.

**Note**  
You can run only file, cached volume, and tape gateways on an Amazon EC2 instance\.

**To choose a host platform and download the VM**

1. On the **Select host platform** page, choose the virtualization platform that you want to run your gateway on\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSelectHostPlatform.png)  
  


1. Do one of the following:
   + If you choose the hardware appliance, activate it by following the instructions in [Activating Your Hardware Appliance](appliance-activation.md)\.
   + If you choose one of the other options, choose **Download image** next to your virtualization platform to download a \.zip file that contains the \.ova file for your virtualization platform\.
**Note**  
The \.zip file is over 500 MB in size and might take some time to download, depending on your network connection\. 

     For Amazon EC2, you create an instance from the provided AMI\.

1. If you choose a hypervisor option, deploy the downloaded image to your hypervisor\. Add at least one local disk for your cache and one local disk for your upload buffer during the deployment\. A file gateway requires only one local disk for a cache\. For information about local disk requirements, see [Hardware and storage requirements](Requirements.md#requirements-hardware-storage)\.

   Depending your hypervisor, set certain options:
   + If you choose VMware, do the following:
     + Store your disk using the **Thick provisioned format** option\. When you use thick provisioning, the disk storage is allocated immediately, resulting in better performance\. In contrast, thin provisioning allocates storage on demand\. On\-demand allocation can affect the normal functioning of AWS Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in thick\-provisioned format\.
     + Configure your gateway VM to use paravirtualized disk controllers\. For more information, see [Configuring the AWS Storage Gateway VM to Use Paravirtualized Disk Controllers](configure-vmware.md#SetParaVirtualization-common)\.
   + If you choose Microsoft Hyper\-V, do the following: 
     + Configure the disk type using the **Fixed size** option\. When you use fixed\-size provisioning, the disk storage is allocated immediately, resulting in better performance\. If you don't use fixed\-size provisioning, the storage is allocated on demand\. On\-demand allocation can affect the functioning of Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in fixed\-size provisioned format\. 
     + When allocating disks, choose **virtual hard disk \(\.vhd\) file**\. Storage Gateway supports the \.vhdx file type\. By using this file type, you can create larger virtual disks than with other file types\. If you create a \.vhdx type virtual disk, make sure that the size of the virtual disks that you create doesn't exceed the recommended disk size for your gateway\.
   + If you choose Linux Kernel\-bases Virtual Machine \(KVM\), do the following: 
     + Don't configure your disk to use `sparse` formatting\. When you use fixed\-size \(nonsparse\) provisioning, the disk storage is allocated immediately, resulting in better performance\.
     + Use the parameter `sparse=false` to store your disk in nonsparse format when creating new virtual disks in the VM with the `virt-install` command for provisioning new virtual machines\.
     + Use `virtio` drivers for disk and network devices\.
     + We recommend that you don't set the `current_memory` option\. If necessary, set it equal to the RAM provisioned to the gateway in the `--ram` parameter\.

     Following is an example `virt-install` command for installing KVM\.

     ```
     virt-install --name "SGW_KVM" --description "SGW KVM" --os-type=generic --ram=32768 --vcpus=16 --disk path=fgw-kvm.qcow2,bus=virtio,size=80,sparse=false --disk path=fgw-kvm-cache.qcow2,bus=virtio,size=1024,sparse=false --network default,model=virtio --graphics none --import
     ```

**Note**  
For VMware, Microsoft Hyper\-V, and KVM, synchronizing the VM time with the host time is required for successful gateway activation\. Make sure that your host clock is set to the correct time and synchronize it with a Network Time Protocol \(NTP\) server\. 

For information about deploying your gateway to an Amazon EC2 host, see [Deploy your gateway to an Amazon EC2 host](ec2-gateway-common.md)\.

## Choosing a Service Endpoint<a name="GettingStarted-service-endpoint-tape"></a>

You can activate your gateway using a public endpoint and have your gateway communicate with AWS storage services over the public internet\. Or you can activate your gateway using a virtual private cloud \(VPC\) endpoint, which is private\. If you use a VPC endpoint, all communication from your gateway to AWS services occurs through the VPC endpoint in your VPC in AWS\.

**To choose a service endpoint**

1. For **Endpoint type**, choose from the following options:
   + To have your gateway access AWS services over the public internet, choose **Public**\.
   + To have your gateway access AWS services over the public internet that complies with Federal Information Processing Standards \(FIPS\), choose **FIPS**\.
   + To have your gateway access AWS services through the VPC endpoint in your VPC, choose **VPC**\.
**Note**  
The FIPS service endpoint is only available in the AWS GovCloud \(US\) Regions\. For more information about FIPS, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

   This procedure assumes that you are activating your gateway with a public endpoint\. For information about how to activate a gateway using a VPC endpoint, see [Activating a Gateway in a Virtual Private Cloud](gateway-private-link.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/choose-endpoint-type.png)

1. Choose **Next** to connect you gateway and activate your gateway\.

## Connecting to Your Gateway<a name="GettingStartedBeginActivateGateway-vtl"></a>

To connect to your gateway, first get the IP address of your gateway VM\. You use this IP address to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address from the Amazon EC2 console\.

The activation process associates your gateway with your AWS account\. Your gateway VM must be running for activation to succeed\.

Make sure that you select the correct gateway type\. The \.ova files and Amazon Machine Images \(AMIs\) for the gateway types are different and are not interchangeable\.

**To get the IP address for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi – [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + Linux KVM – [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. Get the IP address from the top of the menu page, and note it for later use\.

**To get the IP address from an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose the EC2 instance\.

1. Choose the **Description** tab at the bottom, and then note the IP address\. You use this IP address to activate the gateway\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/step7c-get-ip-address.png)

For activation, you can use the public or private IP address assigned to a gateway\. You must be able to reach the IP address that you use from the browser from which you perform the activation\. In this procedure, we use the public IP address to activate the gateway\.

**To associate your gateway with your AWS account**

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Type the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BeginActivation.png)

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.

## Activating Your Gateway<a name="GettingStartedActivateGateway-common"></a>

When your gateway VM is deployed and running, you can configure your gateway settings and activate your gateway\. If activation fails, check that the IP address you entered is correct\. If the IP address is correct, confirm that your network is configured to let your browser access the gateway VM\. For more information on troubleshooting, see [Troubleshooting on\-premises gateway issues](troubleshooting-on-premises-gateway-issues.md) or [Troubleshooting Amazon EC2 gateway issues](troubleshooting-EC2-gateway-issues.md)\.

**To configure your gateway settings**

1. Locate the gateway type, endpoint type, and AWS Region that you selected, which are shown on the activation page\.

1. Enter the information listed on the activation page to configure your gateway settings and complete the activation process\.

   The following screenshot shows the activation page for tape gateways\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-gateway-vtl.png)
   + **AWS Region** specifies the AWS Region where your gateway will be activated and where your data will be stored\. If **Endpoint type** is **VPC**, the AWS Region should be same as the AWS Region where your VPC endpoint is located\.
   + **Gateway time zone** specifies the time zone to use for your gateway\.
   + **Gateway name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\. 
   + **Backup application** specifies the backup application you want to use\. Storage Gateway automatically chooses a compatible medium changer for your backup application\. If your backup application is not listed, choose **Other** and choose a medium changer type\. **Medium changer type** specifies the type of medium changer to use for your backup application\.

     The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with tape gateways\. This table includes the medium changer type recommended for each backup application\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/create-gateway-vtl.html)
**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](resource_vtl-devices.md#change-mediumchanger-vtl)\.
   + **Tape drive type** specifies the type of tape drive used by this gateway\. 

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your gateway\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your gateway\. 

1. Choose **Activate gateway**\.

   When the gateway is successfully activated, the AWS Storage Gateway console displays the **Configure local storage** page\.

   If activation fails, see [Troubleshooting your gateway](troubleshooting-gateway-issues.md) for possible solutions\.

## Configuring Local Disks<a name="configure-local-disk-alarms"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\.

**Note**  
If you allocate local disks on a VMware host, make sure to configure the disks to use paravirtualized disk controllers\.  
When adding a cache or upload buffer to an existing gateway, make sure to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.<a name="local-storage-cached"></a>

**To configure local disks**

1. On the **Configure local disks** page, identify the disks you allocated and decide which ones you want to use for an upload buffer and cached storage\. For information about disk size quotas, see [Recommended local disk sizes for your gateway](resource-gateway-limits.md#disk-sizes)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-local-storage.png)

1. In the **Allocation** column next to your upload buffer disk, choose **Upload Buffer**\.

1. Choose **Cache** for the disk you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

## Configuring Amazon CloudWatch Logging<a name="configure-logging-tape"></a>

To notify you about the health of your tape gateway and its resources, you can configure an Amazon CloudWatch log group\. For more information, see [Getting Tape Gateway Health Logs with CloudWatch Log Groups](GatewayMetrics-vtl-common.md#cw-log-groups-tape)\.

**To configure a CloudWatch log group for your tape gateway**

1. Choose the **Create new Log Group** link to create a new log group\. You are directed to the CloudWatch console to create one\. If you already have a CloudWatch log group that you want to use to monitor your gateway, choose that group for **Gateway Log Group**\.

1. If you create a new log group, choose the refresh button to view the new log group in the list\.

1. If your gateway is deployed on a VMware host that is enabled for VMware High Availability \(HA\) cluster, you're prompted to verify and test the VMware HA configuration\. In this case, choose **Verify VMware HA**\. Otherwise, choose **Save and Continue**\.

## Verify VMware High Availability \(VMware HA Clusters Only\)<a name="verify-ha-tape"></a>

If your gateway is not deployed on a VMware host that is enabled for VMware High Availability \(HA\), you can skip this section\.

If your gateway is deployed on a VMware host that is enabled for VMware High Availability \(HA\) cluster, you can either test the configuration when activating the gateway or after your gateway is activated\. The following instructions show you how to test the configuration during activation\.

**To test for VMware HA**

1. On the Verify VMware High Availability Configuration page, choose **Verify VMware HA**\. This can take up to two minutes to complete\.

1. If the test is successful, a message that indicates a successful test is displayed in the banner\. If the test fails, a failed message is displayed\. You can make changes in your vSphere configuration and repeat the test\.

1. To repeat the test, choose **Actions** and then choose **Verify VMware HA**\.

For information about how to configure your gateway for VMware HA, see [Using VMware vSphere High Availability with AWS Storage Gateway](Performance.md#vmware-ha)\.

**Next Step**

[Creating Tapes](GettingStartedCreateTapes.md)