# Activate the gateway<a name="GettingStartedActivateGateway-file"></a>

The following, shown on the activation page, are the gateway settings that you selected\. The activation page appears after you associate your gateway with your AWS account, as described preceding\.
+ **Gateway type** specifies the type of gateway that you are activating\.
+ **Endpoint type** specifies the type of endpoint that you selected for your gateway\.
+ **AWS Region** specifies the Region where your gateway will be activated and where your data will be stored\. If **Endpoint type** is **VPC**, the AWS Region should be same as the Region where your VPC endpoint is located\.

**Note**  
You can also activate your gateway in a virtual private cloud to create a private connection between your on\-premises software appliance and cloud\-based storage infrastructure\. You can then use the software appliance to transfer data to AWS storage without your gateway communicating with AWS storage services over the public internet\. For instructions, see [Activating a gateway in a virtual private cloud](gateway-private-link.md)\.

------
#### [ New console ]

**To activate your gateway**

1. In **Activate gateway**, do the following:
   + For **Gateway time zone**, select a time zone to use for your gateway\.
   + For **Gateway name**, enter a name to identify your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\.
**Note**  
The gateway name must be between 2 and 255 characters in length\.

1. \(Optional\) For **Add tags**, enter a key and value to add tags to your gateway\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your gateway\.

1. Choose **Activate gateway**\.

------
#### [ Original console ]

**To activate your gateway**

1. To complete the activation process, provide information on the activation page to configure your gateway setting:
   + **Gateway time zone** specifies the time zone to use for your gateway\.
   + **Gateway name** identifies your gateway\. You use this name to manage your gateway in the console; you can change it after the gateway is activated\. This name must be unique to your account\.

1. \(Optional\) In the **Add tags** section, enter a key and value to add tags to your gateway\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your gateway\.

1. Choose **Activate gateway**\.

------

If activation isn't successful, see [Troubleshooting your gateway](troubleshooting-gateway-issues.md) for possible solutions\.