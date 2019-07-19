# Configuring an IP Address for the Gateway<a name="appliance-configure-ip"></a>

To assign a static IP address to a gateway installed on your hardware appliance, configure the IP address from the local console of that gateway\. Your applications \(such as your NFS or SMB client, your iSCSI initiator, and so on\) connect to this IP address\. You can access the gateway local console from the hardware appliance console\. 

**To configure an IP address on your appliance to work with applications**

1. On the hardware console, choose **Open Service Console** to open a login screen for the gateway local console\.

1. Enter the localhost **login** password, and then press `Enter`\.

   For File Gateway the default account is `admin` and the default password is `password`\. For Tape Gateway and Volume Gateway the default account is `sguser` the default password is `sgpassword`\.

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

**Next Step**

[Configuring Your Gateway](appliance-configure-gateway.md)