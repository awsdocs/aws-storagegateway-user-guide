# Rack\-Mount Your Hardware Appliance and Connect It to Power<a name="appliance-rack-mount"></a>

After you unbox your AWS Storage Gateway Hardware Appliance, follow the instructions contained in the box to rack\-mount the server\. Your appliance has a 1U form factor and fits into a 19\-inch rack to the International Electrotechnical Commission \(IEC\) industry standard, as described on the [19\-inch rack](https://en.wikipedia.org/wiki/19-inch_rack) Wikipedia page\.

To install your hardware appliance, you need the following components:
+ Power cables: one required, two recommended\.
+ Category 6 \(Cat6\) Ethernet cable\. A Category 5 \(Cat5\) Ethernet cable limits your throughput\.
+ Keyboard and monitor, or a keyboard, video, and mouse \(KVM\) switch solution\.

**To connect the hardware appliance to power**
**Note**  
Before you perform the following procedure, make sure that you meet all of the requirements for the AWS Storage Gateway Hardware Appliance as described in [Networking and Firewall Requirements for the Hardware Appliance](Requirements.md#appliance-network-requirements)\.

1. Plug in a power connection to each of the two power supplies\. It's possible to plug in to only one power connection, but we recommend power connections to both power supplies\.

   In the following image, you can see the hardware appliance with the different connections\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceBack.png)  
  


1. Plug an Ethernet cable into the `em1` port to provide an always\-on internet connection\. The `em1` port is the first of the four physical network ports on the rear, from left to right\.

1. Plug in the keyboard and monitor\.

1. Power on the server by pressing the **Power** button on the front panel, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceFront.png)  
  


After the server boots up, the hardware console appears on the monitor\. The hardware console presents a user interface specific to AWS that you can use to configure initial network parameters\. You configure these parameters to connect the appliance to AWS and open up a support channel for troubleshooting by AWS Support\.

To work with the hardware console, enter text from the keyboard and use the `Up`, `Down`, `Right`, and `Left Arrow` keys to move about the screen in the indicated direction\. Use the `Tab` key to move sequentially forward through items on\-screen\. On some setups, you can use the `Shift+Tab` keystroke to move sequentially backward\. Use the `Enter` key to save selections, or to choose a button on the screen\.

**To set a password for the first time**

1. For **Set Password**, enter a password, and then press `Down arrow`\.

1. For **Confirm**, re\-enter your password, and then choose **Save Password**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceSetPassword.png)





At this point, you are in the hardware console, shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ApplianceHardwareConsole.png)





**Next Step**

[Configure Network Parameters](appliance-configure-network.md)