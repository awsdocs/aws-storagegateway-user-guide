# Activate Your Hardware Appliance<a name="appliance-activation"></a>

After configuring your IP address, you enter this IP address in the console on the **Hardware** page, as described following\. The activation process validates that your hardware appliance has the appropriate security credentials and registers the appliance to your AWS account\.

AWS Storage Gateway Hardware Appliance is only available in the US and Europe\. You can choose to activate your hardware appliance in any of the supported AWS Regions\. For the supported AWS Regions, see [AWS Storage Gateway Hardware Appliance Regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#sg-hardware-appliance) in the *AWS General Reference*\.

**To activate your appliance for the first time or in an AWS Region where you have no gateways deployed**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/) with the account credentials to use to activate your hardware\.

   If this is your first gateway in an AWS Region, you see the splash screen shown following\. After you create a gateway in this AWS Region, this screen no longer displays\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceIntoSplash.png)  
  

**Note**  
For activation only, the following must be true:   
Your browser must be on the same network as your hardware appliance\.
Your firewall must allow HTTP access on port 8080 to the appliance for inbound traffic\.

1. Choose **Get started** to view the Create gateway wizard, and then choose **Hardware Appliance** on the **Select host platform** page, as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSelectHostPlatform.png)  
  


1. Choose **Next** to view the **Connect to hardware** screen shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceConnectHardware.png)  
  


1. For **IP Address**, enter the IPv4 address of your appliance, and then choose **Connect to Hardware** to go to the Activate Hardware screen shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceActivateHardware.png)  
  


1. For **Hardware name**, enter a name for your appliance\. Names can be up to 255 characters long and can't include a slash character\. 

1. \(Optional\) For **Hardware time zone**, enter your local settings\.

   The time zone controls when hardware updates take place, with 2 a\.m\. local time used as the time for updates\.
**Note**  
We recommend setting the time zone for your appliance as this determines a standard update time that is out of the usual working day window\.

1. \(Optional\) Keep the **RAID Volume Manager** set to **ZFS**\.

   ZFS RAID is a software\-based, open\-source file system and logical volume manager\. We recommend using ZFS for most hardware appliance use cases because it offers superior performance and integration compared with MD RAID\. The hardware appliance is specifically tuned for ZFS RAID\. For more information on ZFS RAID, see the [ZFS](https://en.wikipedia.org/wiki/ZFS) Wikipedia page\. 

   If you don't want to accept CDDL license terms, as documented in [CDDL 1\.0](https://opensource.org/licenses/CDDL-1.0) on the Opensource\.org site, we also offer MD RAID\. For more information on MD RAID, see the [mdadm]( https://en.wikipedia.org/wiki/Mdadm) Wikipedia page\. To change the volume manager on your hardware appliance, contact [AWS Support](https://aws.amazon.com/contact-us)\. AWS Support can provide an International Organization for Standardization \(ISO\) standard image, instructions on performing a factory reset of a hardware appliance, and instructions on installing the new ISO image\. 

1.  Choose **Next** to finish activation\. 

A console banner appears on the Hardware page indicating that the hardware appliance has been successfully activated, as shown following\. 

At this point, the appliance is associated with your account\. The next step is to launch a file, tape, or cached volume gateway on your appliance\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceActivationFinal.png)





**Next Step**

[Launching a Gateway](appliance-launch-gateway.md)