# Troubleshooting hardware appliance issues<a name="troubleshooting-hardware-appliance-issues"></a>

The following topics discuss issues that you might encounter with the Storage Gateway Hardware Appliance, and suggestions on troubleshooting these\.

## You can't determine the service IP address<a name="service_ip_address"></a>

When attempting to connect to your service, make sure that you are using the service's IP address and not the host IP address\. Configure the service IP address in the service console, and the host IP address in the hardware console\. You see the hardware console when you start the hardware appliance\. To go to the service console from the hardware console, choose **Open Service Console**\.

## How do you perform a factory reset?<a name="factory_reset"></a>

If you need to perform a factory reset on your appliance, contact the Storage Gateway Hardware Appliance team for support, as described in the Support section following\.

## Where do you obtain Dell iDRAC support?<a name="iDRAC_support"></a>

The Dell PowerEdge R640 server comes with the Dell iDRAC management interface\. We recommend the following:
+ If you use the iDRAC management interface, you should change the default password\. For more information about the iDRAC credentials, see [Dell PowerEdge \- What is the default username and password for iDRAC?](https://www.dell.com/support/article/en-us/sln306783/dell-poweredge-what-is-the-default-username-and-password-for-idrac?lang=en)\.
+ Make sure that the firmware is up\-to\-date to prevent security breaches\.
+ Moving the iDRAC network interface to a normal \(`em`\) port can cause performance issues or prevent the normal functioning of the appliance\.

## You can't find the hardware appliance serial number<a name="appliance_serial_number"></a>

To find the serial number of the hardware appliance, go to the **Hardware** page in the Storage Gateway console, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/appliance-serial-number.png)





## Where to obtain hardware appliance support<a name="appliance_support"></a>

To contact the Storage Gateway Hardware Appliance support, see [AWS Support](http://aws.amazon.com/contact-us)\.

The AWS Support team might ask you to activate the support channel to troubleshoot your gateway issues remotely\. You don't need this port to be open for the normal operation of your gateway, but it is required for troubleshooting\. You can activate the support channel from the hardware console as shown in the procedure following\.

**To open a support channel for AWS**

1. Open the hardware console\.

1. Choose **Open Support Channel** as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/OpenSupportChannel.png)  
  


   The assigned port number should appear within 30 seconds, if there are no network connectivity or firewall issues\.

1. Note the port number and provide it to AWS Support\.