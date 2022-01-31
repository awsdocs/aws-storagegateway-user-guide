--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Rack\-mounting your hardware appliance and connecting it to power<a name="appliance-rack-mount"></a>

After you unbox your Storage Gateway Hardware Appliance, follow the instructions contained in the box to rack\-mount the server\. Your appliance has a 1U form factor and fits in a standard International Electrotechnical Commission \(IEC\) compliant 19\-inch rack\.

To install your hardware appliance, you need the following components:
+ Power cables: one required, two recommended\.
+ Supported network cabling \(depending on which Network Interface Card \(NIC\) is included in the hardware appliance\)\. Twinax Copper DAC, SFP\+ optical module \(Intel compatible\) or SFP to Base\-T copper transceiver\.
+ Keyboard and monitor, or a keyboard, video, and mouse \(KVM\) switch solution\.

## Hardware appliance dimensions<a name="appliance-dimensions"></a>

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hardware-dimensions.png)





**To connect the hardware appliance to power**
**Note**  
Before you perform the following procedure, make sure that you meet all of the requirements for the Storage Gateway Hardware Appliance as described in [Networking and firewall requirements for the Storage Gateway Hardware Appliance](Requirements.md#appliance-network-requirements)\.

1. Plug in a power connection to each of the two power supplies\. It's possible to plug in to only one power connection, but we recommend power connections to both power supplies\.

   In the following image, you can see the hardware appliance with the different connections\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceBack.png)  
  


1. Plug an Ethernet cable into the `em1` port to provide an always\-on internet connection\. The `em1` port is the first of the four physical network ports on the rear, from left to right\.
**Note**  
The hardware appliance doesn't support VLAN trunking\. Set up the switch port to which you are connecting the hardware appliance as a non\-trunked VLAN port\.

1. Plug in the keyboard and monitor\.

1. Power on the server by pressing the **Power** button on the front panel, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/appliance-front.jpeg)  
  


After the server boots up, the hardware console appears on the monitor\. The hardware console presents a user interface specific to AWS that you can use to configure initial network parameters\. You configure these parameters to connect the appliance to AWS and open up a support channel for troubleshooting by Amazon Web Services Support\.

To work with the hardware console, enter text from the keyboard and use the `Up`, `Down`, `Right`, and `Left Arrow` keys to move about the screen in the indicated direction\. Use the `Tab` key to move forward in order through items on\-screen\. On some setups, you can use the `Shift+Tab` keystroke to move sequentially backward\. Use the `Enter` key to save selections, or to choose a button on the screen\.

**To set a password for the first time**

1. For **Set Password**, enter a password, and then press `Down arrow`\.

1. For **Confirm**, re\-enter your password, and then choose **Save Password**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSetPassword.png)





At this point, you are in the hardware console, shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceHardwareConsole.png)





**Next step**

[Configuring network parameters](appliance-configure-network.md)