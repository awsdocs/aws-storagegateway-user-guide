# Troubleshooting Amazon EC2 gateway issues<a name="troubleshooting-EC2-gateway-issues"></a>

In the following sections, you can find typical issues that you might encounter working with your gateway deployed on Amazon EC2\. For more information about the difference between an on\-premises gateway and a gateway deployed in Amazon EC2, see [Deploying a file gateway on an Amazon EC2 host](ec2-gateway-file.md)\.

For information about using ephemeral storage, see [Using ephemeral storage with EC2 gateways](ManagingLocalStorage-common.md#ephemeral-disk-cache)\.

**Topics**
+ [Your gateway activation hasn't occurred after a few moments](#activation-issues)
+ [You can't find your EC2 gateway instance in the instance list](#find-instance)
+ [You want AWS Support to help troubleshoot your EC2 gateway](#EC2-EnableAWSSupportAccess)

## Your gateway activation hasn't occurred after a few moments<a name="activation-issues"></a>

Check the following in the Amazon EC2 console:
+ Port 80 is enabled in the security group that you associated with the instance\. For more information about adding a security group rule, see [Adding a security group rule](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html#adding-security-group-rule) in the *Amazon EC2 User Guide for Linux Instances*\.
+ The gateway instance is marked as running\. In the Amazon EC2 console, the **State** value for the instance should be RUNNING\.
+ Make sure that your Amazon EC2 instance type meets the minimum requirements, as described in [Storage requirements](Requirements.md#requirements-storage)\.

After correcting the problem, try activating the gateway again\. To do this, open the Storage Gateway console, choose **Deploy a new Gateway on Amazon EC2**, and re\-enter the IP address of the instance\.

## You can't find your EC2 gateway instance in the instance list<a name="find-instance"></a>

If you didn't give your instance a resource tag and you have many instances running, it can be hard to tell which instance you launched\. In this case, you can take the following actions to find the gateway instance:
+ Check the name of the Amazon Machine Image \(AMI\) on the **Description** tab of the instance\. An instance based on the Storage Gateway AMI should start with the text **aws\-storage\-gateway\-ami**\.
+ If you have several instances based on the Storage Gateway AMI, check the instance launch time to find the correct instance\.

## You want AWS Support to help troubleshoot your EC2 gateway<a name="EC2-EnableAWSSupportAccess"></a>

Storage Gateway provides a local console you can use to perform several maintenance tasks, including enabling AWS Support to access your gateway to assist you with troubleshooting gateway issues\. By default, AWS Support access to your gateway is disabled\. You enable this access through the Amazon EC2 local console\. You log in to the Amazon EC2 local console through a Secure Shell \(SSH\)\. To successfully log in through SSH, your instance's security group must have a rule that opens TCP port 22\.

**Note**  
If you add a new rule to an existing security group, the new rule applies to all instances that use that security group\. For more information about security groups and how to add a security group rule, see [Amazon EC2 security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide*\.

To let AWS Support connect to your gateway, you first log in to the local console for the Amazon EC2 instance, navigate to the storage gateway's console, and then provide the access\.

**To enable AWS Support access to a gateway deployed on an Amazon EC2 instance**

1. Log in to the local console for your Amazon EC2 instance\. For instructions, go to [Connect to your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide*\.

   You can use the following command to log in to the EC2 instance's local console\.

   ```
   ssh –i PRIVATE-KEY admin@INSTANCE-PUBLIC-DNS-NAME
   ```
**Note**  
The *PRIVATE\-KEY* is the `.pem` file containing the private certificate of the EC2 key pair that you used to launch the Amazon EC2 instance\. For more information, see [Retrieving the public key for your key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#retriving-the-public-key) in the *Amazon EC2 User Guide*\.  
The *INSTANCE\-PUBLIC\-DNS\-NAME* is the public Domain Name System \(DNS\) name of your Amazon EC2 instance that your gateway is running on\. You obtain this public DNS name by selecting the Amazon EC2 instance in the EC2 console and clicking the **Description** tab\.

   The local console looks like the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/EC2_LocalConsole-StartPage.png)

1. At the prompt, enter **3** to open the AWS Support Channel console\.

1. Enter **h** to open the **AVAILABLE COMMANDS** window\.

1. Do one of the following:
   + If your gateway is using a public endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel** to connect to customer support for Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.
   + If your gateway is using a VPC endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel**\. If your gateway is not activated, provide the VPC endpoint or IP address to connect to customer support for Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/EC2-assign-service-number.png)
**Note**  
The channel number is not a Transmission Control Protocol/User Datagram Protocol \(TCP/UDP\) port number\. Instead, the gateway makes a Secure Shell \(SSH\) \(TCP 22\) connection to Storage Gateway servers and provides the support channel for the connection\.

1. After the support channel is established, provide your support service number to AWS Support so AWS Support can provide troubleshooting assistance\.

1. When the support session is completed, enter **q** to end it\. Don't close the session until Amazon Web Services Support notifies you that the support session is complete\.

1. Enter **exit** to exit the Storage Gateway console\.

1. Follow the console menus to log out of the Storage Gateway instance\.