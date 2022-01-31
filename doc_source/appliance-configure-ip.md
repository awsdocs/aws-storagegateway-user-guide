--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Configuring an IP address for the gateway<a name="appliance-configure-ip"></a>

Before you activated your hardware appliance, you assigned an IP address to its physical network interface\. Now that you have activated the appliance and launched your Storage Gateway on it, you need to assign another IP address to the Storage Gateway virtual machine that runs on the hardware appliance\. To assign a static IP address to a gateway installed on your hardware appliance, configure the IP address from the local console for that gateway\. Your applications \(such as your NFS or SMB client, your iSCSI initiator, and so on\) connect to this IP address\. You can access the gateway local console from the hardware appliance console\.

**To configure an IP address on your appliance to work with applications**

1. On the hardware console, choose **Open Service Console** to open a login screen for the gateway local console\.

1. Enter the localhost **login** password, and then press `Enter`\.

   The default account is `admin` and the default password is `password`\.

1. Change the default password\. Choose **Actions** then **Set Local Password** and enter your new credentials in the **Set Local Password** dialog box\.

1. \(Optional\) Configure your proxy settings\. See [Setting the Local Console Password from the Storage Gateway Console](manage-on-premises-common.md#set-password) for instructions\.

1. Navigate to the Network Settings page of the gateway local console as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceNetworkSettings.png)  
  


1. Type `2` to go to the **Network Configuration** page shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceNetworkConfiguration.png)  
  


1. Configure a static or DHCP IP address for the network port on your hardware appliance to present a file, volume, and tape gateway for applications\. This IP address must be on the same subnet as the IP address used during hardware appliance activation\.

**To exit the gateway local console**
+ Press the `Crtl+]` \(close bracket\) keystroke\. The hardware console appears\.
**Note**  
The keystroke preceding is the only way to exit the gateway local console\.

**Next step**

[Configuring your gateway](appliance-configure-gateway.md)