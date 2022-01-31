--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Setting up your hardware appliance<a name="appliance-quick-start"></a>

After you receive your Storage Gateway Hardware Appliance, you use the hardware appliance console to configure networking to provide an always\-on connection to AWS and activate your appliance\. Activation associates your appliance with the Amazon Web Services account that is used during the activation process\. After the appliance is activated, you can launch a file, volume, or tape gateway from the Storage Gateway console\.

**Note**  
It is your responsibility to ensure the hardware appliance firmware is up\-to\-date\.

**To install and configure your hardware appliance**

1. Rack\-mount the appliance, and plug in power and network connections\. For more information, see [Rack\-mounting your hardware appliance and connecting it to power](appliance-rack-mount.md)\.

1. Set the Internet Protocol version 4 \(IPv4\) addresses for both the hardware appliance \(the host\) and Storage Gateway \(the service\)\. For more information, see [Configuring network parameters](appliance-configure-network.md)\.

1. Activate the hardware appliance on the console **Hardware** page in the AWS Region of your choice\. For more information, see [Activating your hardware appliance](appliance-activation.md)\.

1. Install the Storage Gateway on your hardware appliance\. For more information, see [Configuring your gateway](appliance-configure-gateway.md)\.

   You set up gateways on your hardware appliance the same way that you set up gateways on VMware ESXi, Microsoft Hyper\-V, Linux Kernel\-based Virtual Machine \(KVM\), or Amazon EC2\.

**Increasing the usable cache storage**  
You can increase the usable storage on the hardware appliance from 5 TB to 12 TB\. Doing this provides a larger cache for low latency access to data in AWS\. If you ordered the 5 TB model, you can increase the usable storage to 12 TB by buying five 1\.92 TB SSDs \(solid state drives\), which are available for ordering on the console **Hardware** page\. You can order the additional SSDs by following the same ordering process as ordering a hardware appliance and requesting a sales quote from the Storage Gateway console\.

You can then add them to the hardware appliance before you activate it\. If you have already activated the hardware appliance and want to increase the usable storage on the appliance to 12 TB, do the following:

1. Reset the hardware appliance to its factory settings\. Contact Amazon Web Services Support for instructions on how to do this\.

1. Add five 1\.92 TB SSDs to the appliance\.

**Network interface card options**  
Depending on the model of appliance you ordered, it may come with a 10G\-Base\-T copper network card or a 10G DA/SFP\+ network card\.
+ 10G\-Base\-T NIC configuration:
  + Use CAT6 cables for 10G or CAT5\(e\) for 1G
+ 10G DA/SFP\+ NIC configuration:
  + Use Twinax copper Direct Attach Cables up to 5 meters
  + Dell/Intel compatible SFP\+ optical modules \(SR or LR\)
  + SFP/SFP\+ copper transceiver for 1G\-Base\-T or 10G\-Base\-T