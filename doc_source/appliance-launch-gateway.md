# Launching a Gateway<a name="appliance-launch-gateway"></a>

You can launch any of the three storage gateways on the appliance—file gateway, volume gateway \(cached\), or tape gateway\.

**To launch a gateway on your hardware appliance**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Hardware**\.

1. For **Actions**, choose **Launch Gateway**\.

1. For **Gateway Type**, choose **File Gateway**, **Tape Gateway**, or **Volume Gateway \(Cached\)**\.

1. For **Gateway name**, enter a name for your gateway\. Names can be 255 characters long and can't include a slash character\. 

1. Choose **Launch gateway**\.

The Storage Gateway software for your chosen gateway type installs on the appliance\. It can take up to 5–10 minutes for a gateway to show up as **online** in the console\.

To assign a static IP address to your installed gateway, you next configure the gateway's network interfaces so your applications can use it\. 

**Next Step**

[Configuring an IP Address for the Gateway](appliance-configure-ip.md)