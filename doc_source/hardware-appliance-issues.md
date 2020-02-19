# Troubleshooting Hardware Appliance Issues<a name="hardware-appliance-issues"></a>

The following topics discuss issues that you might encounter with the AWS Storage Gateway Hardware Appliance, and suggestions on troubleshooting these\.

## You Can't Determine the Service IP Address<a name="service_ip_address"></a>

When attempting to connect to your service, make sure that you are using the service's IP address and not the host IP address\. Configure the service IP address in the service console, and the host IP address in the hardware console\. You see the hardware console when you start the hardware appliance\. To go to the service console from the hardware console, choose **Open Service Console**\.

## How Do You Perform a Factory Reset?<a name="factory_reset"></a>

If you need to perform a factory reset on your appliance, contact the AWS Storage Gateway Hardware Appliance team for support, as described in the Support section following\.

## Where Do You Obtain Dell iDRAC Support?<a name="iDRAC_support"></a>

The Dell PowerEdge R640 server comes with the Dell iDRAC management interface\. If you use the iDRAC, then you should change the default password for the iDRAC\. Make sure that the firmware is up\-to\-date to prevent security breaches\. Moving the iDRAC network interface to a normal \(`em`\) port can cause performance issues or prevent the normal functioning of the appliance\.

## You Can't Find the Hardware Appliance Serial Number<a name="appliance_serial_number"></a>

To find the serial number of the hardware appliance, go to the **Hardware** page in the AWS Storage Gateway console, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSerialNumber.png)





## Where to Obtain Hardware Appliance Support<a name="appliance_support"></a>

To contact the AWS Storage Gateway Hardware Appliance support, see [AWS Support](http://aws.amazon.com/contact-us)\.

The AWS Support team might ask you to activate the support channel to troubleshoot your gateway issues remotely\. You don't need this port to be open for the normal operation of your gateway, but it is required for troubleshooting\. You can activate the support channel from the hardware console as shown in the procedure following\.

**To open a support channel for AWS**

1. Open the hardware console\.

1. Choose **Open Support Channel** as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/OpenSupportChannel.png)  
  


   The assigned port number should appear within 30 seconds, if there are no network connectivity or firewall issues\.

1. Note the port number and provide it to AWS Support\. 