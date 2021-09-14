# Troubleshooting Amazon EC2 gateway issues<a name="troubleshooting-EC2-gateway-issues"></a>

In the following sections, you can find typical issues that you might encounter working with your gateway deployed on Amazon EC2\. For more information about the difference between an on\-premises gateway and a gateway deployed in Amazon EC2, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. For information about using ephemeral storage, see [Using Ephemeral Storage With EC2 Gateways](ManagingLocalStorage-common.md#ephemeral-disk-cache)\.

**Topics**
+ [Your gateway activation hasn't occurred after a few moments](#activation-issues)
+ [You can't find your EC2 gateway instance in the instance list](#find-instance)
+ [You created an Amazon EBS volume but can't attach it to your EC2 gateway instance](#ebs-volume-issue)
+ [You can't attach an initiator to a volume target of your EC2 gateway](#initiator-issue)
+ [You get a message that you have no disks available when you try to add storage volumes](#no-disk)
+ [You want to remove a disk allocated as upload buffer space to reduce upload buffer space](#uploadbuffer-issue)
+ [Throughput to or from your EC2 gateway drops to zero](#gateway-throughput-issue)
+ [You want AWS Support to help troubleshoot your EC2 gateway](#EC2-EnableAWSSupportAccess)

## Your gateway activation hasn't occurred after a few moments<a name="activation-issues"></a>

Check the following in the Amazon EC2 console:
+ Port 80 is enabled in the security group that you associated with the instance\. For more information about adding a security group rule, see [Adding a security group rule](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html#adding-security-group-rule) in the *Amazon EC2 User Guide for Linux Instances*\.
+ The gateway instance is marked as running\. In the Amazon EC2 console, the **State** value for the instance should be RUNNING\.
+ Make sure that your Amazon EC2 instance type meets the minimum requirements, as described in [Storage requirements](Requirements.md#requirements-storage)\.

After correcting the problem, try activating the gateway again\. To do this, open the AWS Storage Gateway console, choose **Deploy a new Gateway on Amazon EC2**, and re\-enter the IP address of the instance\.

## You can't find your EC2 gateway instance in the instance list<a name="find-instance"></a>

If you didn't give your instance a resource tag and you have many instances running, it can be hard to tell which instance you launched\. In this case, you can take the following actions to find the gateway instance:
+ Check the name of the Amazon Machine Image \(AMI\) on the **Description** tab of the instance\. An instance based on the AWS Storage Gateway AMI should start with the text **aws\-storage\-gateway\-ami**\.
+ If you have several instances based on the AWS Storage Gateway AMI, check the instance launch time to find the correct instance\.

## You created an Amazon EBS volume but can't attach it to your EC2 gateway instance<a name="ebs-volume-issue"></a>

Check that the Amazon EBS volume in question is in the same Availability Zone as the gateway instance\. If there is a discrepancy in Availability Zones, create a new Amazon EBS volume in the same Availability Zone as your instance\.

## You can't attach an initiator to a volume target of your EC2 gateway<a name="initiator-issue"></a>

Check that the security group that you launched the instance with includes a rule that allows the port that you are using for iSCSI access\. The port is usually set as 3260\. For more information on connecting to volumes, see [Connecting to Your Volumes to a Windows Client](initiator-connection-common.md#ConfiguringiSCSIClient)\.

## You get a message that you have no disks available when you try to add storage volumes<a name="no-disk"></a>

For a newly activated gateway, no volume storage is defined\. Before you can define volume storage, you must allocate local disks to the gateway to use as an upload buffer and cache storage\. For a gateway deployed to Amazon EC2, the local disks are Amazon EBS volumes attached to the instance\. This error message likely occurs because no Amazon EBS volumes are defined for the instance\.

Check block devices defined for the instance that is running the gateway\. If there are only two block devices \(the default devices that come with the AMI\), then you should add storage\. For more information on doing so, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. After attaching two or more Amazon EBS volumes, try creating volume storage on the gateway\.

## You want to remove a disk allocated as upload buffer space to reduce upload buffer space<a name="uploadbuffer-issue"></a>

Follow the steps in [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.

## Throughput to or from your EC2 gateway drops to zero<a name="gateway-throughput-issue"></a>

Verify that the gateway instance is running\. If the instance is starting due to a reboot, for example, wait for the instance to restart\.

Also, verify that the gateway IP has not changed\. If the instance was stopped and then restarted, the IP address of the instance might have changed\. In this case, you need to activate a new gateway\.

You can view the throughput to and from your gateway from the Amazon CloudWatch console\. For more information about measuring throughput to and from your gateway to AWS, see [Measuring Performance Between Your Gateway and AWS](monitoring-volume-gateway.md#PerfGatewayAWS-common)\.

## You want AWS Support to help troubleshoot your EC2 gateway<a name="EC2-EnableAWSSupportAccess"></a>

AWS Storage Gateway provides a local console you can use to perform several maintenance tasks, including enabling AWS Support to access your gateway to assist you with troubleshooting gateway issues\. By default, AWS Support access to your gateway is disabled\. You enable this access through the Amazon EC2 local console\. You log in to the Amazon EC2 local console through a Secure Shell \(SSH\)\. To successfully log in through SSH, your instance's security group must have a rule that opens TCP port 22\.

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
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

1. At the prompt, enter **3** to open the AWS Support Channel console\.

1. Enter **h** to open the **AVAILABLE COMMANDS** window\.

1. Do one of the following:
   + If your gateway is using a public endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel** to connect to customer support for AWS Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.
   + If your gateway is using a VPC endpoint, in the **AVAILABLE COMMANDS** window, enter **open\-support\-channel**\. If your gateway is not activated, provide the VPC endpoint or IP address to connect to customer support for Storage Gateway\. Allow TCP port 22 so you can open a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2-assign-service-number.png)
**Note**  
The channel number is not a Transmission Control Protocol/User Datagram Protocol \(TCP/UDP\) port number\. Instead, the gateway makes a Secure Shell \(SSH\) \(TCP 22\) connection to Storage Gateway servers and provides the support channel for the connection\.

1. After the support channel is established, provide your support service number to AWS Support so AWS Support can provide troubleshooting assistance\.

1. When the support session is completed, enter **q** to end it\.

1. Enter **exit** to exit the AWS Storage Gateway console\.

1. Follow the console menus to log out of the AWS Storage Gateway instance\.