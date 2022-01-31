--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Activating a gateway in a virtual private cloud<a name="gateway-private-link"></a>

You can create a private connection between your on\-premises software appliance and cloud\-based storage infrastructure\. You can then use the software appliance to transfer data to AWS storage without your gateway communicating with AWS storage services over the public internet\. Using the Amazon VPC service, you can launch AWS resources in a custom virtual network\. You can use a virtual private cloud \(VPC\) to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

To use a gateway with a Storage Gateway VPC endpoint in your VPC, do the following:
+ Use the VPC console to create a VPC endpoint for Storage Gateway and get the VPC endpoint ID\. Specify this VPC endpoint ID when you create and activate the gateway\.
+ If you are activating a file gateway, create a VPC endpoint for Amazon S3\. Specify this VPC endpoint when you create file shares for your gateway\.
+ If you are activating a file gateway, set up a HTTP proxy and configure it in the file gateway VM local console\. You need this proxy for on\-premises file gateways that are hypervisor\-based, such as those based on VMware, Microsoft HyperV, and Linux Kernel\-based Virtual Machine \(KVM\) \. In these cases, you need the proxy to enable your gateway access Amazon S3 private endpoints from outside your VPC\. For information about how to configure a HTTP proxy, see [Configuring an HTTP proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)\.

**Note**  
Your gateway must be activated in the same region where your VPC endpoint was created\.  
For file gateway, the Amazon S3 storage that is configured for the file share must be in the same region where you created the VPC endpoint for Amazon S3\.

**Topics**
+ [Creating a VPC endpoint for Storage Gateway](#create-vpc-endpoint)
+ [Setting up and configuring an HTTP proxy \(on\-premises file gateways only\)](#create-proxy-file-vpc)
+ [Allowing traffic to required ports in your HTTP proxy](#required-proxy-ports)

## Creating a VPC endpoint for Storage Gateway<a name="create-vpc-endpoint"></a>

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

## Setting up and configuring an HTTP proxy \(on\-premises file gateways only\)<a name="create-proxy-file-vpc"></a>

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

1. Log in to your gateway's VM local console\. For information about how to log in, see [Logging in to the file gateway local console](manage-on-premises-fgw.md#LocalConsole-login-fgw)\.

1. In the main menu, choose **Configure HTTP proxy**\.

1. In the **Configuration** menu, choose **Configure HTTP proxy**\.

1. Provide the host name and port for your proxy server\.

For detailed information on how to configure a HTTP proxy, see [Configuring an HTTP proxy](manage-on-premises-fgw.md#MaintenanceRoutingProxy-fgw)\.

## Allowing traffic to required ports in your HTTP proxy<a name="required-proxy-ports"></a>

If you use a HTTP proxy, make sure that you allow traffic from Storage Gateway to the destinations and ports listed following\.

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