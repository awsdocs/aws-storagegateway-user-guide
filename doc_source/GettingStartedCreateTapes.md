# Creating Tapes<a name="GettingStartedCreateTapes"></a>

**Note**  
You are charged only for the amount of data you write to the tape, not the tape capacity\.

**To create virtual tapes**

1. In the navigation pane, choose the **Gateways** tab\.

1. Choose **Create tapes** to open the **Create tape** dialog box\.

1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

1. For **Number of tapes**, choose the number of tapes you want to create\. For more information about tape limits, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\.

1. For **Capacity**, type the size of the virtual tape you want to create\. Tapes must be larger than 100 GiB\. For information about capacity limits, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\.

1. For **Barcode prefix**, type the prefix you want to prepend to the barcode of your virtual tapes\. 
**Note**  
Virtual tapes are uniquely identified by a barcode\. You can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

1. Choose **Create tapes**\.

1. In the navigation pane, choose the **Tapes** tab to see your tapes\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/created-tapes.png)

The status of the virtual tapes is initially set to **CREATING** when the virtual tapes are being created\. After the tapes are created, their status changes to **AVAILABLE**\. For more information, see [Managing Your Tape Gateway](managing-gateway-vtl.md)\.

**Next Step**

[Using Your Tape Gateway](GettingStarted-create-tape-gateway.md)