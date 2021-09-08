# Activating a gateway in a virtual private cloud<a name="gateway-private-link"></a>

You can create a private connection between your on\-premises gateway and cloud\-based storage infrastructure\. You can then use the gateway to transfer data to AWS storage without your gateway communicating with AWS storage services over the public internet\. Using the Amazon VPC service, you can launch AWS resources in a custom virtual network\. You can use a virtual private cloud \(VPC\) to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

To use a gateway with a Storage Gateway VPC endpoint in your VPC, do the following:
+ Use the VPC console to create a VPC endpoint for Storage Gateway and get the VPC endpoint ID\. You use this VPC endpoint ID to activate the gateway\.
+ If you are setting up a file gateway, create a VPC interface endpoint for Amazon S3 and get the VPC endpoint ID\. You use this VPC endpoint ID when you configure file shares for your gateway after it is activated\. For instructions to configure a file share, see [Create an NFS file share](https://docs.aws.amazon.com/filegateway/latest/files3/CreatingAnNFSFileShare.html) or [Create an SMB file share](https://docs.aws.amazon.com/filegateway/latest/files3/CreatingAnSMBFileShare.html)\.

**Note**  
Your gateway must be activated in the same region where your VPC endpoint was created\.  
For file gateway, the Amazon S3 bucket that is configured for the file share must be in the same region where you created the VPC endpoint for S3\.

## Creating a gateway using a VPC endpoint<a name="create-gateway-file-vpc"></a>

In this section, you can find instructions about how to download, deploy, and activate your file gateway using a VPC endpoint\.

**Topics**
+ [Creating a VPC endpoint for Storage Gateway](#create-vpc-endpoint)
+ [Choosing a gateway type](#GettingStartedSelectGatewayType-file-vpc)
+ [Choosing a host platform and downloading the VM](#hosting-options-file-vpc)
+ [Choosing a service endpoint](#GettingStarted-service-endpoint-file-vpc)
+ [Connecting to your gateway](#GettingStartedBeginActivateGateway-file-vpc)
+ [Activate your gateway in a VPC](#GettingStartedActivateGateway-file-vpc)
+ [Configure local disks](#configure-local-storage-alarms-file-vpc)
+ [Allowing traffic to required ports in your HTTP proxy](#required-proxy-ports)

### Creating a VPC endpoint for Storage Gateway<a name="create-vpc-endpoint"></a>

Follow these instructions to create a VPC endpoint\. If you already have a VPC endpoint for Storage Gateway, you can use it\.<a name="create-vpc-steps"></a>

**To create a VPC endpoint for Storage Gateway**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, choose **AWS Services** for **Service category**\.

1. For **Service Name**, choose `com.amazonaws.region.storagegateway`\. For example `com.amazonaws.us-east-2.storagegateway`\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is not selected\.

1. For **Security group**, choose the security group that you want to use for your VPC\. You can accept the default security group\. Verify that all of the following TCP ports are allowed in your security group:
   + TCP 443
   + TCP 1026
   + TCP 1027
   + TCP 1028
   + TCP 1031
   + TCP 2222

1. Choose **Create endpoint**\. The initial state of the endpoint is **pending**\. When the endpoint is created, note the ID of the VPC endpoint that you just created\.

1. When the endpoint is created, choose **Endpoints**, then choose the new VPC endpoint\.

1. In the **DNS Names** section, use the first DNS name that doesn't specify an Availability Zone\. Your DNS name look similar to this: `vpce-1234567e1c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com `

Now that you have a VPC endpoint, you can create your gateway\.

**Important**  
If you are creating file gateway, you need to create an endpoint for Amazon S3 also\. Follow the same steps as shown in To create a VPC endpoint for Storage Gateway section above but you choose `com.amazonaws.us-east-2.s3` under Service Name instead\. Then you select the route table that you want the S3 endpoint associated with instead of subnet/security group\. For instructions, see [Creating a gateway endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint)\.

### Choosing a gateway type<a name="GettingStartedSelectGatewayType-file-vpc"></a>

**To choose a gateway type**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the service homepage appears\.

1. If you haven't created a gateway in the AWS Region that you chose, choose **Get started**\. If you already have a gateway in the AWS Region that you chose, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. For **Select gateway type**, choose a gateway type, and then choose **Next**\.

### Choosing a host platform and downloading the VM<a name="hosting-options-file-vpc"></a>

If you create your gateway on premises, you deploy the hardware appliance, or download and deploy a gateway VM, and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported hypervisors and host requirements](Requirements.md#requirements-host)\.

**Note**  
You can run only file, cached volume, and tape gateways on an Amazon EC2 instance\.

**To choose a host platform and download the VM**

1. For **Select host platform**, choose the virtualization platform that you want to run your gateway on\.

1. Do one of the following:
   + If you choose the hardware appliance, activate it by following the instructions in [Activating your hardware appliance](appliance-activation.md)\.
   + If you choose one of the other options, choose **Download image** next to your virtualization platform to download a \.zip file that contains the \.ova file for your virtualization platform\.
**Note**  
The \.zip file is over 500 MB in size and might take some time to download, depending on your network connection\. 

     For Amazon EC2, you create an instance from the provided AMI\.

1. If you choose a hypervisor option, deploy the downloaded image to your hypervisor\. Add at least one local disk for your cache and one local disk for your upload buffer during the deployment\. A file gateway requires only one local disk for a cache\. For information about local disk requirements, see [Hardware and storage requirements](Requirements.md#requirements-hardware-storage)\.

   Depending on your hypervisor, you can set specific options:
   + If you choose VMware, do the following:
     + Store your disk using the **Thick provisioned format** option\. When you use thick provisioning, the disk storage is allocated immediately, resulting in better performance\. In contrast, thin provisioning allocates storage on demand\. On\-demand allocation can affect the normal functioning of Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in thick\-provisioned format\.
   + If you choose Microsoft Hyper\-V, do the following: 
     + Configure the disk type using the **Fixed size** option\. When you use fixed\-size provisioning, the disk storage is allocated immediately, resulting in better performance\. If you don't use fixed\-size provisioning, the storage is allocated on demand\. On\-demand allocation can affect the functioning of Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in fixed\-size provisioned format\. 
     + When allocating disks, choose **virtual hard disk \(\.vhd\) file**\. Storage Gateway supports the \.vhdx file type\. By using this file type, you can create larger virtual disks than with other file types\. If you create a \.vhdx type virtual disk, make sure that the size of the virtual disks that you create doesn't exceed the recommended disk size for your gateway\.
   + If you choose Linux Kernel\-based Virtual Machine \(KVM\), do the following: 
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

For information about deploying your gateway to an Amazon EC2 host, see [Deploy your gateway to an Amazon EC2 host](ec2-gateway-file.md)\.

### Choosing a service endpoint<a name="GettingStarted-service-endpoint-file-vpc"></a>

You can activate your gateway using a private VPC endpoint\. If you use a VPC endpoint, all communication from your gateway to AWS services occurs through the VPC endpoint in your VPC in AWS\.

------
#### [ New Console ]

**To choose a service endpoint**

1. For **Select service endpoint**, choose **VPC**\.

1. If you don't have a VPC endpoint, choose **Create a VPC endpoint** to create one in the Amazon VPC console\. For instructions on the creating a VPC endpoint, see [Creating a VPC endpoint for Storage Gateway](#create-vpc-endpoint)\. A VPC endpoint allows your gateway to communicate with AWS services only through your VPC in AWS without going over the public internet\.

1. In **VPC endpoint**, enter the DNS name or the IP address of the VPC endpoint for Storage Gateway\. The DNS name looks similar to this: `vpce-1234567e1c11a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`\.

1. If you already have a VPC endpoint, choose **Amazon VPC endpoints**\. You can identify an existing VPC endpoint by it's DNS name, IP address or VCP endpoint ID\.

1. To identify the VPC endpoint by DNS name, choose **DNS name \(recommended\) or IP address**, provide the DNS name or the IP address\. The DNS name looks similar to this: `vpce-1234567e1c11a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`\.

1. To identify the VPC endpoint by VPC endpoint ID, choose **VPC endpoint ID** and choose the ID you want from the list\. 

1. Choose **Next** to connect and activate your gateway\.

------
#### [ Original Console ]

**To choose a service endpoint**

1. In the console, you can select a service endpoint for your gateway after selecting the host platform\. For **Endpoint type**, choose **VPC**\. If you don't have a VPC endpoint, choose **Create a VPC endpoint** to create one\. A VPC endpoint allows your gateway to communicate with AWS services only through your VPC in AWS without going over the public internet\.

   This procedure assumes that you are activating your gateway using a VPC endpoint\. For more information about how to activate a gateway using a public endpoint, see the following topics:
   + [Getting started with Storage Gateway](create-file-gateway.md)

1. Enter the VPC endpoint DNS name for Storage Gateway that you created in the [Creating a VPC endpoint for Storage Gateway](#create-vpc-endpoint) section\. Your DNS name looks similar to this: `vpce-1234567e1c11a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`\.

1. Choose **Next** to connect to your gateway and activate your gateway\.

------

### Connecting to your gateway<a name="GettingStartedBeginActivateGateway-file-vpc"></a>

To connect to your gateway, first get the IP address or activation key of your gateway VM\. You use the IP address or activation key to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address or activation key from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address or activation key from the Amazon EC2 console\.

The activation process associates your gateway with your Amazon Web Services account\. Your gateway VM must be running for activation to succeed\.

**Note**  
Make sure that you select the correct gateway type\. The \.ova files and Amazon Machine Images \(AMIs\) for the gateway types are different and are not interchangeable\.

**To get the IP address or activation key for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi – \.
   + Microsoft Hyper\-V – \.
   + Linux KVM – \.

1. Get the IP address from the top of the menu page, and note it for later use\.

**To get the IP address or activation key from an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose the EC2 instance\.

1. Choose the **Details** tab at the bottom, and then note the IP address or activation key\. You use one of these to activate the gateway\.

**Note**  
For activation with an IP address, you can use the public or private IP address assigned to a gateway\. You must be able to reach the IP address that you use from the browser from which you perform the activation\.

------
#### [ New Console ]

**To associate your gateway with your Amazon Web Services account**

1. For **Connect to gateway**, choose one of the following:
   + **IP address**
   + **Activation key**

1. Enter the IP address or activation key of your gateway, and then choose **Next**\.

------
#### [ Original Console ]

**To associate your gateway with your Amazon Web Services account**

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Type the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.

------

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.

### Activate your gateway in a VPC<a name="GettingStartedActivateGateway-file-vpc"></a>

Activating a file gateway requires additional setup\.

The following, shown on the activation page, are the gateway settings that you selected\. The activation page appears after you associate your gateway with your Amazon Web Services account, as described preceding\.
+ **Gateway type** specifies the type of gateway that you are activating\.
+ **Endpoint type** specifies the type of endpoint that you selected for your gateway\.
+ **AWS Region** specifies the AWS Region where your gateway will be activated and where your data will be stored\. If **Endpoint type** is **VPC**, the AWS Region should be same as the Region where your VPC endpoint is located\.

------
#### [ New Console ]

**To activate your gateway**

1. In **Activate gateway**, do the following:
   + For **Gateway time zone**, select a time zone to use for your gateway\.
   + For **Gateway name**, enter a name to identify your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\.
**Note**  
The gateway name must be between 2 and 255 characters in length\.

1. \(Optional\) For **Add tags**, enter a key and value to add tags to your gateway\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your gateway\.

1. Choose **Activate gateway**\.

------
#### [ Original Console ]

**To activate your gateway**

1. To complete the activation process, provide information on the activation page to configure your gateway setting:
   + **Gateway Time Zone** specifies the time zone to use for your gateway\.
   + **Gateway Name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\.

1. Choose **Activate gateway**\.

------

If activation isn't successful, see [Troubleshooting your gateway](troubleshooting-gateway-issues.md) for possible solutions\.

**To associate your gateway with your Amazon Web Services account**

If you don’t have internet access and private network access from your browser, you can still do the following\.

1. Enter the fully qualified DNS name of the VPC endpoint or elastic network interface to get the activation key from the gateway\. You can use curl with the following URL, or just enter this URL into your web browser\.

   `http://VM IP ADDRESS/?gatewayType=FILE_S3&activationRegion=REGION&vpcEndpoint=VPCEndpointDNSname&no_redirect`

   An example curl command follows\.

   `curl "http://203.0.113.100/?gatewayType=FILE_S3&activationRegion=us-east-1&vpcEndpoint=vpce-12345678e91c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com&no_redirect"`

   An example activation key follows\.

   `BME11-LQPTD-DF11P-BLLQ0-111V1`

1. Use the AWS CLI to activate the gateway by specifying the activation key you received in previous step, for example:

    `aws --region us-east-1 storagegateway activate-gateway --activation-key BME11-LQPTD-DF11P-BLLQ0-111V1 --gateway-type FILE_S3 --gateway-name user-ec2-iad-pl-fgw2 --gateway-timezone GMT-4:00 --gateway-region us-east-1 --endpoint-url https://vpce-12345678e91c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`

   Following is an example response\.

   ```
   {"GatewayARN": "arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-FFF12345"}
   ```

### Configure local disks<a name="configure-local-storage-alarms-file-vpc"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\.<a name="local-storage-cached-file-vpc"></a>

**To configure local disks**

1. For **Configure local disks**, identify the disks you added and decide which ones you want to allocate for cached storage\. We recommend minimum cache size of 150 GiB and Maximum 64 TiB\.

1. For **Allocated to**, choose **Cache** for the disk that you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

### Allowing traffic to required ports in your HTTP proxy<a name="required-proxy-ports"></a>

Using an HTTP proxy is optional\. If you use an HTTP proxy, make sure that you allow traffic from Storage Gateway to the destinations and ports listed following\.

When Storage Gateway is communicating through the public endpoints, it communicates with the following Storage Gateway services\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
storagegateway.region.amazonaws.com:443 (Required for making API calls)
s3.region.amazonaws.com (Required only for File Gateway)
```

**Important**  
Depending on your gateway's AWS Region, replace *region* in the endpoint with the corresponding region string\. For example, if you create a gateway in the US West \(Oregon\) region, the endpoint looks like this: `storagegateway.us-west-2.amazonaws.com:443`\.

When Storage Gateway is communicating through the VPC endpoint, it communicates with the AWS services through multiple ports on the Storage Gateway VPC endpoint and port 443 on the Amazon S3 private endpoint\.
+ TCP ports on Storage Gateway VPC endpoint\.
  + 443, 1026, 1027, 1028, 1031, and 2222
+ TCP port on S3 private endpoint
  + 443

You are now ready to create resources for your gateway\.

**Next Step**

S3 File [Create a file share](https://docs.aws.amazon.com/filegateway/latest/files3/GettingStartedCreateFileShare.html)