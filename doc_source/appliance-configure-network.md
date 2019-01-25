# Configure Network Parameters<a name="appliance-configure-network"></a>

After the server boots up, you can enter your first password in the hardware console as described in [Rack\-Mount Your Hardware Appliance and Connect It to Power](appliance-rack-mount.md)\. 

Next, on the hardware console take the following steps to configure network parameters so your hardware appliance can connect to AWS\.

**To set a network address**

1. Choose **Configure Network** and press the `Enter` key\. The **Configure Network** screen shown following appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceConfigureNetwork.png)  
  


1. For **IP Address**, enter a valid IPv4 address from one of the following sources: 
   + Use the IPv4 address assigned by your Dynamic Host Configuration Protocol \(DHCP\) server to your physical network port\.

     If you do so, note this IPv4 address for later use in the activation step\. 
   + Assign a static IPv4 address\. To do so, choose **Static** in the `em1` section and press `Enter` to view the Configure Static IP screen shown following\.

     The `em1` section is at upper left section in the group of port settings\.

   After you have entered a valid IPv4 address, press the `Down arrow` or `Tab`\.
**Note**  
If you configure any other interface, it must provide the same always\-on connection to the AWS endpoints listed in the requirements\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceStaticIP.png)  
  


1. For **Subnet**, enter a valid subnet mask, and then press `Down arrow`\.

1. For **Gateway**, enter your network gateway’s IPv4 address, and then press `Down arrow`\.

1. For **DNS1**, enter the IPv4 address for your Domain Name Service \(DNS\) server, and then press `Down arrow`\.

1. \(Optional\) For **DNS2**, enter a second IPv4 address, and then press `Down arrow`\. A second DNS server assignment would provide additional redundancy should the first DNS server become unavailable\.

1. Choose **Save** and then press `Enter` to save your static IPv4 address setting for the appliance\.

**To log out of the hardware console**

1. Choose **Back** to return to the Main screen\.

1. Choose **Logout** to return to the Login screen\.

**Next Step**

[Activate Your Appliance](appliance-activation.md)