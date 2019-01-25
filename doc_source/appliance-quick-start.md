# Setting Up Your Hardware Appliance<a name="appliance-quick-start"></a>

After you receive your AWS Storage Gateway Hardware Appliance, you use the hardware appliance console to configure networking to provide an always\-on connection to AWS and activate your appliance\. Activation associates your appliance with the AWS account that is used during the activation process\. After the appliance activates, you can launch a file, volume, or tape gateway in the Storage Gateway console\.

To install and configure your hardware appliance, do the following:

1. Rack\-mount the appliance, and plug in power and network connections\. For more information, see [Rack\-Mount Your Hardware Appliance and Connect It to Power](appliance-rack-mount.md)\.

1. Set the Internet Protocol version 4 \(IPv4\) addresses for both the hardware appliance \(the host\) and Storage Gateway \(the service\)\. For more information, see [Configure Network Parameters](appliance-configure-network.md)\.

1. Activate the hardware appliance on the console **Hardware** page in the AWS Region of your choice\. For more information, see [Activate Your Appliance](appliance-activation.md)\.

1. Install the Storage Gateway service on your hardware appliance\. For more information, see [Configure Your Gateway](appliance-configure-gateway.md)\.

   You set up gateways on your hardware appliance the same way that you set up gateways on a VMware ESXi or Microsoft Hyper\-V hypervisor or an Amazon EC2 instance\. 