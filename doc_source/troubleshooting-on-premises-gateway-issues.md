# Troubleshooting on\-premises gateway issues<a name="troubleshooting-on-premises-gateway-issues"></a>

You can find information following about typical issues that you might encounter working with your on\-premises gateways, and how to enable AWS Support to help troubleshoot your gateway\.

The following table lists typical issues that you might encounter working with your on\-premises gateways\.


| Issue | Action to Take | 
| --- | --- | 
| You cannot find the IP address of your gateway\.  |  Use the hypervisor client to connect to your host to find the gateway IP address\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshooting-on-premises-gateway-issues.html) If you are still having trouble finding the gateway IP address: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshooting-on-premises-gateway-issues.html)  | 
| You're having network or firewall problems\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshooting-on-premises-gateway-issues.html)  | 
|  Your gateway's activation fails when you click the **Proceed to Activation** button in the AWS Storage Gateway Management Console\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshooting-on-premises-gateway-issues.html)  | 
| You need to remove a disk allocated as upload buffer space\. For example, you might want to reduce the amount of upload buffer space for a gateway, or you might need to replace a disk used as an upload buffer that has failed\.  | For instructions about removing a disk allocated as upload buffer space, see [Volume Gateway](resource-volume-gateway.md)\.  | 
|  You need to improve bandwidth between your gateway and AWS\.  |  You can improve the bandwidth from your gateway to AWS by setting up your internet connection to AWS on a network adapter \(NIC\) separate from that connecting your applications and the gateway VM\. Taking this approach is useful if you have a high\-bandwidth connection to AWS and you want to avoid bandwidth contention, especially during a snapshot restore\. For high\-throughput workload needs, you can use [AWS Direct Connect](http://aws.amazon.com/directconnect/) to establish a dedicated network connection between your on\-premises gateway and AWS\. To measure the bandwidth of the connection from your gateway to AWS, use the `CloudBytesDownloaded` and `CloudBytesUploaded` metrics of the gateway\. For more on this subject, see [Measuring Performance Between Your Gateway and AWS](monitoring-volume-gateway.md#PerfGatewayAWS-common)\. Improving your internet connectivity helps to ensure that your upload buffer does not fill up\.  | 
|  Throughput to or from your gateway drops to zero\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshooting-on-premises-gateway-issues.html) You can view the throughput to and from your gateway from the Amazon CloudWatch console\. For more information about measuring throughput to and from your gateway to AWS, see [Measuring Performance Between Your Gateway and AWS](monitoring-volume-gateway.md#PerfGatewayAWS-common)\.  | 
|  You are having trouble importing \(deploying\) AWS Storage Gateway on Microsoft Hyper\-V\.  |  See [Troubleshooting Microsoft Hyper\-V setup](troubleshooting-hyperv-setup.md), which discusses some of the common issues of deploying a gateway on Microsoft Hyper\-V\.  | 
|  You receive a message that says: "The data that has been written to the volume in your gateway isn't securely stored at AWS"\.  |  You receive this message if your gateway VM was created from a clone or snapshot of another gateway VM\. If this isn’t the case, contact AWS Support\.  | 

## Enabling AWS Support to help troubleshoot your gateway hosted on\-premises<a name="enable-support-access-on-premises"></a>

AWS Storage Gateway provides a local console you can use to perform several maintenance tasks, including enabling AWS Support to access your gateway to assist you with troubleshooting gateway issues\. By default, AWS Support access to your gateway is disabled\. You enable this access through the host's local console\. To give AWS Support access to your gateway, you first log in to the local console for the host, navigate to the storage gateway's console, and then connect to the support server\.

**To enable AWS Supportaccess to your gateway**

1. Log in to your host's local console\.
   + VMware ESXi – for more information, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – for more information, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.

   The local console looks like the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/LocalConsoleLogin.png)

1. At the prompt, enter **5** to open the AWS Support Channel console\.

1. Enter **h** to open the **AVAILABLE COMMANDS** window\.

1. 

   Do one of the following:
   + If your gateway is using a public endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel** to connect to customer support for AWS Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.
   + If your gateway is using a VPC endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel**\. If your gateway is not activated, provide the VPC endpoint or IP address to connect to customer support for Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2-assign-service-number.png)
**Note**  
The channel number is not a Transmission Control Protocol/User Datagram Protocol \(TCP/UDP\) port number\. Instead, the gateway makes a Secure Shell \(SSH\) \(TCP 22\) connection to Storage Gateway servers and provides the support channel for the connection\.

1. After the support channel is established, provide your support service number to AWS Support so AWS Support can provide troubleshooting assistance\.

1. When the support session is completed, enter **q** to end it\.

1. Enter **exit** to log out of the AWS Storage Gateway console\.

1. Follow the prompts to exit the local console\.