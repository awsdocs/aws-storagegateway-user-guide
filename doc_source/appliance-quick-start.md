# Setting Up Your Hardware Appliance<a name="appliance-quick-start"></a>

After you receive your AWS Storage Gateway Hardware Appliance, you use the hardware appliance console to configure networking to provide an always\-on connection to AWS and activate your appliance\. Activation associates your appliance with the AWS account that is used during the activation process\. After the appliance is activated, you can launch a file, volume, or tape gateway types in the AWS Storage Gateway console\.

**To install and configure your hardware appliance**

1. Rack\-mount the appliance, and plug in power and network connections\. For more information, see [Rack\-Mount Your Hardware Appliance and Connect It to Power](appliance-rack-mount.md)\.

1. Set the Internet Protocol version 4 \(IPv4\) addresses for both the hardware appliance \(the host\) and Storage Gateway \(the service\)\. For more information, see [Configure Network Parameters](appliance-configure-network.md)\.

1. Activate the hardware appliance on the console **Hardware** page in the AWS Region of your choice\. For more information, see [Activate Your Hardware Appliance](appliance-activation.md)\.

1. Install the Storage Gateway on your hardware appliance\. For more information, see [Configuring Your Gateway](appliance-configure-gateway.md)\.

   You set up gateways on your hardware appliance the same way that you set up gateways on a VMware ESXi or Microsoft Hyper\-V hypervisor or an Amazon EC2 instance\. 

**Increasing the usable cache storage**  
You can increase the usable storage on the hardware appliance from 5 TB to 12 TB\. This provides a larger cache for low latency access to data in AWS\. To increase the usable storage to 12 TB, you can buy five 1\.92 TB SSDs \(solid state drives\), which is available on the Amazon Website, and add them to the hardware appliance before you activate it\. If you have already activated the hardware appliance and want to increase the usable storage on the appliance to 12 TB, do the following: 

1. First, reset the hardware appliance to its factory settings\. Contact AWS support for instructions on how to do this\.

1. Add five 1\.92 TB SSDs to the appliance\.

For instructions on how to do this, see the [Drives](https://www.dell.com/support/manuals/us/en/04/poweredge-r640/per640_ism_pub/drives?guid=guid-ef972c95-735f-4e14-89ab-ccf3e572232f&lang=en-us) in the *Dell EMCPowerEdgeR640 Installation and Service Manual*\.

**Using a fiber optic network card instead of copper network card**  
The hardware appliance comes with a 10 gigabit copper network card but you can replace it with a 10 gigabit fiber optic network card that AWS Storage Gateway Hardware Appliance supports\. The specific fiber optic network card that the hardware appliance supports is the Dell Intel X710 Quad Port 10GB Da/SFP\+ Network Daughter Card\. You can buy it from the hardware appliance product page on the Amazon website\. For instructions on how to install the card, see, [Network daughter card](https://www.dell.com/support/manuals/us/en/04/poweredge-r640/per640_ism_pub/network-daughter-card?guid=guid-ef4c0ee4-e499-4fe0-b319-c51a56c4622e&lang=en-us) in the *Dell EMCPowerEdgeR640 Installation and Service Manual*\.