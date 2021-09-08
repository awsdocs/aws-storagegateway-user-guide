# Connect to the gateway<a name="GettingStartedBeginActivateGateway-file"></a>

To connect to your gateway, first get the IP address or activation key of your gateway VM\. You use the IP address or activation key to activate your gateway\. For gateways deployed and activated on an on\-premises host, you can get the IP address or activation key from your gateway VM local console or your hypervisor client\. For gateways deployed and activated on an Amazon EC2 instance, you can get the IP address or activation key from the Amazon EC2 console\.

The activation process associates your gateway with your AWS account\. Your gateway VM must be running for activation to succeed\.

**Note**  
Make sure that you select the correct gateway type\. The \.ova files and Amazon Machine Images \(AMIs\) for the gateway types are different and are not interchangeable\.

**To get the IP address or activation key for your gateway VM from the local console**

1. Log on to your gateway VM local console\. For detailed instructions, see the following:
   + VMware ESXi – [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + Linux KVM – [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. Get the IP address from the top of the menu page, and note it for later use\.

**To get the IP address or activation key from an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose the EC2 instance\.

1. Choose the **Details** tab at the bottom, and then note the IP address or activation key\. You use one of these to activate the gateway\.

**Note**  
For activation with an IP address, you can use the public or private IP address assigned to a gateway\. You must be able to reach the IP address that you use from the browser from which you perform the activation\.

------
#### [ New console ]

**To associate your gateway with your AWS account**

1. For **Connect to gateway**, choose one of the following:
   + **IP address**
   + **Activation key**

1. Enter the IP address or activation key of your gateway, and then choose **Next**\.

------
#### [ Original console ]

**To associate your gateway with your AWS account**

1. If the **Connect to gateway** page isn't already open, open the console and navigate to that page\.

1. Enter the IP address of your gateway for **IP address**, and then choose **Connect gateway**\.

------

For detailed information about how to get a gateway IP address, see [Connecting to Your Gateway](getting-ip-address.md)\.