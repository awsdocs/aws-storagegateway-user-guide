# Activating a Gateway in a Virtual Private Cloud<a name="gateway-private-link"></a>

You can create a private connection between your on\-premises software appliance and cloud\-based storage infrastructure\. You can then use the software appliance to transfer data to AWS storage without your gateway communicating with AWS storage services over the public internet\. Using the Amazon VPC service, you can launch AWS resources in a custom virtual network\. You can use a virtual private cloud \(VPC\) to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

To use a gateway with a Storage Gateway VPC endpoint in your VPC, do the following:
+ Use the VPC console to create a VPC endpoint for Storage Gateway and get the VPC endpoint ID\.
+ If you are activating a file gateway, create a VPC endpoint for Amazon S3\.
+ If you are activating a file gateway, set up a HTTP proxy and configure it in the file gateway VM local console\. You need this proxy for on\-premises file gateways that are hypervisor\-based, such as those based on VMware, Microsoft HyperV, and Linux Kernel\-based Virtual Machine \(KVM\) \. In these cases, you need the proxy to enable your gateway access Amazon S3 private endpoints from outside your VPC\. For information about how to configure a HTTP proxy, see [Configuring an HTTP Proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)\.
+ Use the VPC endpoint ID to activate the gateway\. 

**Note**  
Your gateway must be activated in the same region where your VPC endpoint was created\.  
For file gateway, the Amazon S3 that is configured for the file share must be in the same region where you created the VPC endpoint for S3\.

## Creating a Gateway Using a VPC Endpoint<a name="create-gateway-file-vpc"></a>

In this section, you can find instructions about how to download, deploy, and activate your file gateway using a VPC endpoint\. 

**Topics**
+ [Creating a VPC Endpoint for Storage Gateway](#create-vpc-endpoint)
+ [Choosing a Gateway Type](#GettingStartedSelectGatewayType-file-vpc)
+ [Choosing a Host Platform and Downloading the VM](#hosting-options-file-vpc)
+ [Choosing a Service Endpoint](#GettingStarted-service-endpoint-file-vpc)
+ [Connecting to Your Gateway](#GettingStartedBeginActivateGateway-file-vpc)
+ [Activate Your Gateway in a VPC](#GettingStartedActivateGateway-file-vpc)
+ [Configure Local Disks](#configure-local-storage-alarms-file-vpc)
+ [Allowing Traffic to Required Ports in Your HTTP Proxy](#required-proxy-ports)

### Creating a VPC Endpoint for Storage Gateway<a name="create-vpc-endpoint"></a>

Follow these instructions to create a VPC endpoint\. If you already have a VPC endpoint for Storage Gateway, you can use it\.<a name="create-vpc-steps"></a>

**To create a VPC endpoint for AWS Storage Gateway**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, choose **AWS Services** for **Service category**\.

1. For **Service Name**, choose `com.amazonaws.region.storagegateway` \. For example `com.amazonaws.us-east-2.storagegateway`\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is selected\.

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
If you are creating file gateway, you need to create an endpoint for Amazon S3 also\. Follow the same steps as shown in To create a VPC endpoint for AWS Storage Gateway section above but you choose `com.amazonaws.us-east-2.s3` under Service Name instead\. Then you select the route table that you want the S3 endpoint associated with instead of subnet/security group\. For instructions, see [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint)\.

### Choosing a Gateway Type<a name="GettingStartedSelectGatewayType-file-vpc"></a>

**To choose a gateway type**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the service homepage appears\.

1. If you haven't created a gateway in the AWS Region that you chose, choose **Get started**\. If you already have a gateway in the AWS Region that you chose, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. On the **Select gateway type** page, choose a gateway type, and then choose **Next**\. In this example file gateway is selected\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/file-gateway.png)

### Choosing a Host Platform and Downloading the VM<a name="hosting-options-file-vpc"></a>

If you create your gateway on\-premises, you deploy the hardware appliance, or download and deploy a gateway VM, and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported Hypervisors and Host Requirements](Requirements.md#requirements-host)\.

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

1. If you choose a hypervisor option, deploy the downloaded image to your hypervisor\. Add at least one local disk for your cache and one local disk for your upload buffer during the deployment\. A file gateway requires only one local disk for a cache\. For information about local disk requirements, see [Hardware and Storage Requirements](Requirements.md#requirements-hardware-storage)\.

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

For information about deploying your gateway to an Amazon EC2 host, see [Deploy Your Gateway to an Amazon EC2 Host](ec2-gateway-file.md)\.

### Choosing a Service Endpoint<a name="GettingStarted-service-endpoint-file-vpc"></a>

You can activate your gateway using a private VPC endpoint\. If you use a VPC endpoint, all communication from your gateway to AWS services occurs through the VPC endpoint in your VPC in AWS\. 

1. In the console, you can select a service endpoint for your gateway after selecting the host platform\.  For **Endpoint type**, choose **VPC**\. If you don't have a VPC endpoint, choose **Create a VPC endpoint** to create one\. A VPC endpoint allows your gateway to communicate with AWS services only through your VPC in AWS without going over the public internet\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/choose-endpoint-vpc.png)

   This procedure assumes that you are activating your gateway using a VPC endpoint\. For more information about how to activate a gateway using a public endpoint, see [Creating Your Gateway](create-gateways.md)\.

1. Enter the VPC endpoint DNS name for Storage Gateway that you created in the [Creating a VPC Endpoint for Storage Gateway](#create-vpc-endpoint) section\. Your DNS name looks similar to this: `vpce-1234567e1c11a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`\.

1. Choose **Next** to connect to your gateway and activate your gateway\.

### Connecting to Your Gateway<a name="GettingStartedBeginActivateGateway-file-vpc"></a>

To connect to your gateway, first get the IP address of your gateway VM\. You use this IP address to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address from the Amazon EC2 console\.

The activation process associates your gateway with your AWS account\. Your gateway VM must be running for activation to succeed\.

Make sure that you choose the correct gateway type\. The \.ova files and AMIs for the gateway types are different and are not interchangeable\.

**To get the IP address for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi – [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + KVM – [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. Get the IP address from the top of the menu page, and make note of it for later use\.

**To get the IP address from an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose the EC2 instance\.

1. Choose the **Description** tab, and then note the IP address\. You use this IP address to activate the gateway\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/step7c-get-ip-address.png)

For activation, you can use the public or private IP address assigned to a gateway\. You must be able to reach the IP address that you use from the browser from which you perform the activation\. In this procedure, we use the public IP address to activate the gateway\.

#### Setting Up and Configuring a HTTP Proxy \(On\-Premises File Gateways Only\)<a name="create-proxy-file-vpc"></a>

If you are activating a file gateway, you need to set up an HTTP proxy and configure it by using the file gateway VM local console\. You need this proxy for an on\-premises file gateway to access Amazon S3 private endpoints from outside your VPC\. If you already have a HTTP proxy in Amazon EC2, you can use it\. However, you need to verify that all of the following TCP ports are allowed in your security group: 
+ TCP 443
+ TCP 1026
+ TCP 1027
+ TCP 1028
+ TCP 1031
+ TCP 2222

If you don't have an Amazon EC2 proxy, use the following procedure to set up and configure a HTTP proxy\.

**To set up a proxy server**

1. Launch an Amazon EC2 Linux AMI\. We recommend using an instance family that is network\-optimized, such as the c5n\.large\.

1. Use the following command to install squid: **sudo yum install squid**\. Doing this creates a default config file in `/etc/squid/squid.conf`\. 

1. Replace the contents of this config file with the following\.

   ```
   #
   # Recommended minimum configuration:
   #
    
   # Example rule allowing access from your local networks.
   # Adapt to list your (internal) IP networks from where browsing
   # should be allowed
   acl localnet src 10.0.0.0/8              # RFC1918 possible internal network
   acl localnet src 172.16.0.0/12       # RFC1918 possible internal network
   acl localnet src 192.168.0.0/16    # RFC1918 possible internal network
   acl localnet src fc00::/7       # RFC 4193 local private network range
   acl localnet src fe80::/10      # RFC 4291 link-local (directly plugged) machines
    
   acl SSL_ports port 443
   acl SSL_ports port 1026
   acl SSL_ports port 1027
   acl SSL_ports port 1028
   acl SSL_ports port 1031
   acl SSL_ports port 2222
   acl CONNECT method CONNECT
    
   #
   # Recommended minimum Access Permission configuration:
   #
   # Deny requests to certain unsafe ports
   http_access deny !SSL_ports
    
   # Deny CONNECT to other than secure SSL ports
   http_access deny CONNECT !SSL_ports
    
   # Only allow cachemgr access from localhost
   http_access allow localhost manager
   http_access deny manager
    
   # Example rule allowing access from your local networks.
   # Adapt localnet in the ACL section to list your (internal) IP networks
   # from where browsing should be allowed
   http_access allow localnet
   http_access allow localhost
    
   # And finally deny all other access to this proxy
   http_access deny all
    
   # Squid normally listens to port 3128
   http_port 3128
    
   # Leave coredumps in the first cache dir
   coredump_dir /var/spool/squid
    
   #
   # Add any of your own refresh_pattern entries above these.
   #
   refresh_pattern ^ftp:                     1440       20%        10080
   refresh_pattern ^gopher:            1440       0%          1440
   refresh_pattern -i (/cgi-bin/|\?) 0             0%          0
   refresh_pattern .                             0              20%        4320
   ```

1. If you don't need to lock down the proxy server and don't need to make any changes, then enable and start the proxy server using the following commands\. These commands start the server when it boots up\.

   ```
   sudo chkconfig squid on
   sudo service squid start
   ```

You now configure the HTTP proxy for Storage Gateway to use it\. When configuring the gateway to use a proxy, use the default squid port 3128\. The squid\.conf file that is generated covers the following required TCP ports by default:
+ TCP 443
+ TCP 1026
+ TCP 1027
+ TCP 1028
+ TCP 1031
+ TCP 2222

**To use the VM local console to configure the HTTP proxy**

1. Log in to your gateway's VM local console\. For information about how to log in, see [Logging In to the File Gateway Local Console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.

1. In the main menu, choose **Configure HTTP proxy**\.

1. In the **Configuration** menu, choose **Configure HTTP proxy**\.

1. Provide the host name and port for your proxy server\.

For detailed information on how to configure a HTTP proxy, see [Configuring an HTTP Proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)\.

**To associate your gateway with your AWS account**

1. Make sure that you have internet access and also have access to a private network from your browser\.

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Enter the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BeginActivation.png)

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.

### Activate Your Gateway in a VPC<a name="GettingStartedActivateGateway-file-vpc"></a>

Activating a file gateway requires additional setup\.

The gateway type, endpoint type, and AWS Region you selected are shown on the activation page\.

**To activate your gateway**

1. To complete the activation process, provide information on the activation page to configure your gateway setting:
   + **Gateway Time Zone** specifies the time zone to use for your gateway\.
   + **Gateway Name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\. 

     The following screenshot shows the activation page for a file gateway\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-gateway-file-vpc.png)

1. Choose **Activate gateway**\.

1. If activation is not successful, see [Troubleshooting Your Gateway](Troubleshooting-common.md) for possible solutions\.

**To associate your gateway with your AWS account**

If you don’t have internet access and private network access from your browser, you can still do the following\.

1. Enter the fully qualified DNS name of the VPC endpoint or elastic network interface to get the activation key from the gateway\. You can use curl with the following URL, or just enter this URL into your web browser\.

   `http://VM IP ADDRESS/?gatewayType=FILE_S3&activationRegion=REGION&vpcEndpoint=VPCEndpointDNSname&no_redirect`

   An example curl command follows\.

   `curl “http://203.0.113.100/?gatewayType=FILE_S3&activationRegion=us-east-1&vpcEndpoint=vpce-12345678e91c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com&no_redirect”`

   An example activation key follows\.

   `BME11-LQPTD-DF11P-BLLQ0-111V1`

1. Use the AWS CLI to activate the gateway by specifying the activation key you received in previous step, for example:

    `aws --region us-east-1 storagegateway activate-gateway --activation-key BME11-LQPTD-DF11P-BLLQ0-111V1 --gateway-type FILE_S3 --gateway-name user-ec2-iad-pl-fgw2 --gateway-timezone GMT-4:00 --gateway-region us-east-1`

   Following is an example response\.

   ```
   {"GatewayARN": "arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-FFF12345"}
   ```

### Configure Local Disks<a name="configure-local-storage-alarms-file-vpc"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\. <a name="local-storage-cached-file-vpc"></a>

**To configure local disks**

1. On the **Configure local disks** page, identify the disks you added and decide which ones you want to allocate for cached storage\. For information about disk size quotas, see [Recommended Local Disk Sizes For Your Gateway](resource-gateway-limits.md#disk-sizes)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-local-storage-file.png)

1. Choose **Cache** for the disk you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

### Allowing Traffic to Required Ports in Your HTTP Proxy<a name="required-proxy-ports"></a>

If you use a HTTP proxy, make sure that you allow traffic from Storage Gateway to the destinations and ports listed following\.

When Storage Gateway is communicating through the public endpoints, it communicates with the following Storage Gateway services\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
storagegateway.region.amazonaws.com:443 (Required for making API calls)
region.s3.amazonaws.com (Required only for File Gateway)
```

When Storage Gateway is communicating through the VPC endpoint, it communicates with the AWS services through multiple ports on the Storage Gateway VPC endpoint and port 443 on the Amazon S3 private endpoint\.
+ TCP ports on Storage Gateway VPC endpoint\.
  + 443, 1026, 1027, 1028, 1031, and 2222
+ TCP port on S3 private endpoint
  + 443

You are now ready to create resources for your gateway\.

**Next Step**
+ File gateway: [Creating a File Share](GettingStartedCreateFileShare.md)
+ Volume gateway: [Creating a Volume](GettingStartedCreateVolumes.md)
+ Tape gateway: [Creating Tapes](GettingStartedCreateTapes.md)