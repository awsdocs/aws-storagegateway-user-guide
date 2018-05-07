# Connecting to Your Gateway<a name="getting-ip-address"></a>

After you choose a host and deploy your gateway VM, you connect and activate your gateway\. To do this, you need the IP address of your gateway VM\. You get the IP address from your gateway's local console\. You log in to the local console and get the IP address from the top of the console page\.

For gateways deployed on\-premises, you can also get the IP address from your hypervisor\. For Amazon EC2 gateways, you can also get the IP address of your Amazon EC2 instance from the Amazon EC2 Management Console\. To find how to get your gateway's IP address, see one of the following:
+ VMware host: [Accessing the Gateway Local Console with VMware ESXi](manage-on-premises-vmware.md#MaintenanceConsoleWindowVMware-common)
+ HyperV host: [Access the Gateway Local Console with Microsoft Hyper\-V](manage-on-premises-Hyperv.md#MaintenanceConsoleWindowHyperV-common)
+ EC2 host: [Getting an IP Address from an Amazon EC2 Host](#get-ip-from-ec2)

When you locate the IP address, take note of it\. Then return to the AWS Storage Gateway console and type the IP address into the console\.

## Getting an IP Address from an Amazon EC2 Host<a name="get-ip-from-ec2"></a>

To get the IP address of the Amazon EC2 instance your gateway is deployed on, log in to the EC2 instance's local console\. Then get the IP address from the top of the console page\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](ec2-local-console-common.md#EC2_MaintenanceConsoleWindow-common)\.

You can also get the IP address from the Amazon EC2 Management Console\. We recommend using the public IP address for activation\. To get the public IP address, use procedure 1\. If you choose to use the elastic IP address instead, see procedure 2\. <a name="get-ip-ec2-console"></a>

**Procedure 1: To connect to your gateway using the public IP address**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then select the EC2 instance that your gateway is deployed on\.

1. Choose the **Description** tab at the bottom, and then note the public IP\. You use this IP address to connect to the gateway\. Return to the AWS Storage Gateway console and type in the IP address\.

If you want to use the elastic IP address for activation, use the procedure following\.

**Procedure 2: To connect to your gateway using the elastic IP address**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then select the EC2 instance that your gateway is deployed on\.

1. Choose the **Description** tab at the bottom, and then note the **Elastic IP** value\. You use this elastic IP address to connect to the gateway\. Return to the AWS Storage Gateway console and type in the elastic IP address\.

1. After your gateway is activated, choose the gateway that you just activated, and then choose the **VTL devices** tab in the bottom panel\.

1. Get the names of all your VTL devices\.

1. For each target, run the following command to configure the target\.

   `iscsiadm -m node -o new -T [$TARGET_NAME] -p [$Elastic_IP]:3260`

1. For each target, run the following command to log in\.

   `iscsiadm -m node -p [$ELASTIC_IP]:3260 --login`

   Your gateway is now connected using the elastic IP address of the EC2 instance\.