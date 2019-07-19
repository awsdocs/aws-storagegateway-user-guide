# Activating a Gateway in a Virtual Private Cloud<a name="gateway-private-link"></a>

You can create a private connection between your on\-premises software appliance and cloud\-based storage infrastructure\. You can then use the software appliance to transfer data to AWS storage without your gateway communication with AWS storage services over the public internet\. Using Amazon VPC, you can launch AWS resources in a custom virtual network\. You can use a VPC to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

To use a gateway with Storage Gateway VPC endpoint in your VPC, you do the following:
+ Use the VPC console to create a VPC endpoint for Storage Gateway and get the VPC endpoint ID\.
+ If you are activating file gateway, you need to create a VPC endpoint for Amazon S3\.
+ If you are activating a file gateway, you need to setup a http proxy and configure it in the file gateway VM local console\. This proxy is needed for on\-premises VMWare and Microsoft HyperV hypervisor based file gateway, The proxy is required to enable your gateway access Amazon S3 private endpoints from outside your VPC\. For information about how to configure a Http proxy, see [Configuring an HTTP Proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)
+ Use the VPC endpoint ID to activate the gateway\. 

**Note**  
Your gateway must be activated in the same region where your VPC endpoint was created\.  
For file gateway, the Amazon S3 that is configured for the file share must be in the same region where you created the VPC endpoint for S3\.

## Creating a Gateway Using a VPC Endpoint<a name="create-gateway-file-vpc"></a>

In this section, you can find instructions about how to download, deploy, and activate your file gateway using a VPC endpoint\. 

**Topics**
+ [Create VPC Endpoint for Storage Gateway](#create-vpc-endpoint)
+ [Choose a Gateway Type](#GettingStartedSelectGatewayType-file-vpc)
+ [Choose a Host Platform and Downloading the VM](#hosting-options-file-vpc)
+ [Choose a Service Endpoint](#GettingStarted-service-endpoint-file-vpc)
+ [Connect to Your Gateway](#GettingStartedBeginActivateGateway-file-vpc)
+ [Activate Your Gateway in a VPC](#GettingStartedActivateGateway-file-vpc)
+ [Configure Local Disks](#configure-local-storage-alarms-file-vpc)
+ [Allow Traffic to Required Ports in Your HTTP Proxy](#required-proxy-ports)

### Create VPC Endpoint for Storage Gateway<a name="create-vpc-endpoint"></a>

Follow these instructions to create a VPC endpoint\. If you already have a VPC endpoint for Storage Gateway, you can use it\.<a name="create-vpc-steps"></a>

**To create a VPC endpoint for AWS Storage Gateway**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, choose **AWS Services** for **Service category**\.

1. For **Service Name**, choose `com.amazonaws.region.storagegateway`, and then choose **Create endpoint**\. For example `com.amazonaws.us-east-2.storagegateway`\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is selected\.

1. For **Security group**, choose the security group that you want to use for your VPC\. You can accept the default security group\. Verify that all of the following TCP ports are allowed in your security group: 
   + TCP 443
   + TCP 1026
   + TCP 1027
   + TCP 1028
   + TCP 1031
   + TCP 2222

1. Choose **Create endpoint**\. The initial state of the endpoint is **pending**\. When the endpoint is created, take note of the ID of the VPC endpoint that you just created\. 

1. When the endpoint is created, choose endpoints then choose the new VPC endpoint\.

1. Find the DNS Names section and use the first DNS name that does not specify an availability zone\. Your DNS name will look similar to this `vpce-1234567e1c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com `

Now that you have a VPC endpoint, you can create your gateway\.

**Important**  
If you are creating file gateway, you need to create an endpoint for Amazon S3 also\. Follow the same steps as shown in To create a VPC endpoint for AWS Storage Gateway section above but you choose `com.amazonaws.us-east-2.s3` under Service Name instead\. Then you select the route table that you want the S3 endpoint associated with instead of subnet/security group\. For instructions, see [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint)\.

### Choose a Gateway Type<a name="GettingStartedSelectGatewayType-file-vpc"></a>

**To choose a gateway type**

1. Open the AWS Management Console at [http://console\.aws\.amazon\.com/storagegateway/home](http://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the service homepage appears\.

1. If you haven't created a gateway in the AWS Region that you chose, choose **Get started**\. If you already have a gateway in the AWS Region that you chose, choose **Gateways** from the navigation pane, and then choose **Create gateway**\.

1. On the **Select gateway type** page, choose a gateway type, and then choose **Next**\. In this example file gateway is selected\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/file-gateway.png)

### Choose a Host Platform and Downloading the VM<a name="hosting-options-file-vpc"></a>

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

### Choose a Service Endpoint<a name="GettingStarted-service-endpoint-file-vpc"></a>

You can activate your gateway using a private VPC endpoint\. If you use a VPC endpoint, all communication from your gateway to AWS services occurs through the VPC endpoint in your VPC in AWS\. 

1. For **Endpoint type**, choose **VPC**\. If you don't have a VPC endpoint, choose **Create a VPC endpoint** to create one\. A VPC endpoint allows your gateway to communicate with AWS services only through your VPC in AWS without going over the public internet\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/choose-endpoint-vpc.png)

   This walkthorough assumes that you are activating your gateway using a VPC endpoint\. For Information about how to activate a gateway using a public endpoint, see [Creating Your Gateway](create-gateways.md)\.

1. Enter the VPC endpoint DNS name for Storage Gateway you created in the [Create VPC Endpoint for Storage Gateway](#create-vpc-endpoint) section\. Your DNS name will look like this `vpce-1234567e1c11a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`\.

1. Choose **Next** to connect to your gateway and activate your gateway\.

### Connect to Your Gateway<a name="GettingStartedBeginActivateGateway-file-vpc"></a>

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

#### Setup and Configure a HTTP Proxy \(On\-premises File Gateway Only\)<a name="create-proxy-file-vpc"></a>

If you are activating a file gateway, you need to setup a http proxy and configure it in the file gateway VM local console\. This proxy is needed for on\-premises file gateway to access Amazon S3 private endpoints from outside your VPC\. If you already have a http proxy in Amazon EC2, you can use it\. You do, however, need to verify that all of the following TCP ports are allowed in your security group: 
+ TCP 443
+ TCP 1026
+ TCP 1027
+ TCP 1028
+ TCP 1031
+ TCP 2222

If you don't have an Amazon EC2 proxy, follow these steps to setup and configure a http proxy\.

**To setup a proxy server**

1. Launch an Amazon EC2 Linux AMI\. We recommend using an instance family that is network optimized such as the c5n\.large\.

1. Use this command to install squid\. **sudo yum install squid**\. This creates a default config file in `/etc/squid/squid.conf`\. 

1. Replace the contents of this config file with the following:

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

1. If you don't need to lock down the proxy server and don't need to make any changes, then enable and start it using the following commands\. These commands will start the server when it boots up\.

   ```
   sudo chkconfig squid on
   sudo service squid start
   ```

You now configure the http proxy for Storage Gateway to use it\. When configuring the gateway to use a proxy, use the default squid port 3128\. The squid conf that is generated covers the following required TCP ports by default:
+ TCP 443
+ TCP 1026
+ TCP 1027
+ TCP 1028
+ TCP 1031
+ TCP 2222

**To use the VM local console to configure the http proxy, follow these steps**

1. Log in to your gateway's VM local console\. For information about how to log in, see [Logging In to the File Gateway Local Console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.

1. In the main menu, choose **Configure HTTP proxy**\.

1. In the Configuration menu, choose **Configure HTTP proxy**\.

1. Provide the host name and port for your proxy server\.

For detailed information on how to configure a HTTP proxy, see [Configuring an HTTP Proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)\.

**To associate your gateway with your AWS account \(If you have internet access and a private network access from your browser\)**

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Type the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/BeginActivation.png)

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.

### Activate Your Gateway in a VPC<a name="GettingStartedActivateGateway-file-vpc"></a>

Activating a file gateway requires additional setup\.

**To activate your gateway**

The gateway type, endpoint type, and AWS Region you selected are shown on the activation page\.

1. To complete the activation process, provide information on the activation page to configure your gateway setting:
   + **Gateway Time Zone** specifies the time zone to use for your gateway\.
   + **Gateway Name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\. 

     The following screenshot shows the activation page for a file gateway\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/activate-gateway-file-vpc.png)

1. Choose **Activate gateway**\.

1. If activation is not successful, see [Troubleshooting Your Gateway](Troubleshooting-common.md) for possible solutions\.

**To associate your gateway with your AWS account \(If you don’t have internet access and private network access from your browser\)**

1. Enter the fully qualified DNS name of the PL DNS name or ENI to get the activation key from the gateway\. You can use curl against the following URL or just enter it into your web browser\.

   `http://VM IP ADDRESS/?gatewayType=FILE_S3&activationRegion=REGION&vpcEndpoint=VPCEndpointDNSname&no_redirect`

   For example: 

   `curl “http://35.123.456.789/?gatewayType=FILE_S3&activationRegion=us-east-1&vpcEndpoint=vpce-12345678e91c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com&no_redirect”`

   Example activation key:

   `BME11-LQPTD-DF11P-BLLQ0-111V1`

1. Use the CLI to activate the gateway by specifying the activation key you received in previous step\.

   For example:

    `aws --region us-east-1 storagegateway activate-gateway --activation-key BME11-LQPTD-DF11P-BLLQ0-111V1 --gateway-type FILE_S3 --gateway-name user-ec2-iad-pl-fgw2 --gateway-timezone GMT-4:00 --gateway-region us-east-1`

   Example response: 

   ```
   {"GatewayARN": "arn:aws:storagegateway:us-east-1:123456789012:gateway/sgw-FFF12345"}
   ```

### Configure Local Disks<a name="configure-local-storage-alarms-file-vpc"></a>

When you deployed the VM, you allocated local disks for your gateway\. Now you configure your gateway to use these disks\. <a name="local-storage-cached-file-vpc"></a>

**To configure local disks**

1. On the **Configure local disks** page, identify the disks you added and decide which ones you want to allocate for cached storage\. For information about disk size limits, see [Recommended Local Disk Sizes For Your Gateway](resource-gateway-limits.md#disk-sizes)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-local-storage-file.png)

1. Choose **Cache** for the disk you want to configure as cache storage\.

   If you don't see your disks, choose **Refresh**\.

1. Choose **Save and continue** to save your configuration settings\.

### Allow Traffic to Required Ports in Your HTTP Proxy<a name="required-proxy-ports"></a>

If you are using a HTTP Proxy, you need to allow traffic from Storage Gateway to the destinations and ports listed below\.

When Storage Gateway is communicating through the public endpoints, it communicates with the following Storage Gateway services\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
storagegateway.region.amazonaws.com:443 (Required for making API calls)
region.s3.amazonaws.com (Required only for File Gateway)
```

When Storage Gateway is communicating through the VPC endpoint, it communicates with the AWS services through multiple ports on the Storage Gateway VPC endpoint and port 443 on the S3 private endpoint\.
+ TCP ports on Storage Gateway VPC endpoint\.
  + 443, 1026, 1027, 1028, 1031, and 2222
+ TCP port on S3 private endpoint
  + 443

You are now ready to create resources for your gateway\.

**Next Step**
+ File gateway: [Creating a File Share](GettingStartedCreateFileShare.md)
+ Volume gateway: [Creating a Volume](GettingStartedCreateVolumes.md)
+ Tape gateway: [Creating Tapes](GettingStartedCreateTapes.md)