# Troubleshooting Amazon EC2 Gateway Issues<a name="EC2GatewayTroubleshooting"></a>

The following table lists typical issues that you might encounter working with your gateway deployed on Amazon Elastic Compute Cloud \(Amazon EC2\)\. For more information about the difference between an on\-premises gateway and a gateway deployed in Amazon EC2, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. 

**Topics**
+ [Enabling AWS Support To Help Troubleshoot Your Gateway Hosted on an Amazon EC2 Instance](#EC2-EnableAWSSupportAccess)


| Issue | Action to Take | 
| --- | --- | 
| Your Amazon EC2 gateway activation fails when you click the Proceed to Activation button in the AWS Storage Gateway console\.  |  If activation has not occurred in a few moments, check the following in the Amazon EC2 console: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/EC2GatewayTroubleshooting.html) After correcting the problem, try activating the gateway again by going to the AWS Storage Gateway console, clicking **Deploy a new Gateway on Amazon EC2**, and re\-entering the IP address of the instance\.  | 
| You can't find your Amazon EC2 gateway instance in the list of instances\. |  If you did not give your instance a resource tag and you have many instances running, it can be hard to tell which instance you deployed the gateway in\. In this case, you can take the following actions to find the gateway instance: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/EC2GatewayTroubleshooting.html)  | 
| You created an Amazon EBS volume but can't attach it to your Amazon EC2 gateway instance\. |  Check that the Amazon EBS volume in question is in the same Availability Zone as the gateway instance\. If there is a discrepancy in Availability Zones, create a new Amazon EBS volume in the same Availability Zone as your instance\.  | 
| You can't attach an initiator to a volume target of your Amazon EC2 gateway\. |  Check that the security group you launched the instance with includes a rule allowing the port that you are using for iSCSI access\. The port is usually set as 3260\. For more information on connecting to volumes, see [Connecting to Your Volumes to a Windows Client](initiator-connection-common.md#ConfiguringiSCSIClient)\.  | 
| You activated your Amazon EC2 gateway, but when you try to add storage volumes, you receive an error message indicating you have no disks available\. |  For a newly activated gateway, no volume storage is defined\. Before you can define volume storage, you must allocate local disks to the gateway to use as an upload buffer and cache storage\. For a gateway deployed to Amazon EC2, the local disks are Amazon EBS volumes attached to the instance\. This error message likely occurs because no Amazon EBS volumes are defined for the instance\.  Check block devices defined for the instance that is running the gateway\. If there are only two block devices \(the default devices that come with the AMI\), then you should add storage\. For more information on doing so, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. After attaching two or more Amazon EBS volumes, try creating volume storage on the gateway\.  | 
| You need to remove a disk allocated as upload buffer space because you want to reduce the amount of upload buffer space\. |  Follow the steps in [Adding and Removing Upload Buffer](ManagingLocalStorage-common.md#GatewayCachedUploadBuffer)\.  | 
| Throughput to or from your Amazon EC2 gateway drops to zero\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/EC2GatewayTroubleshooting.html) You can view the throughput to and from your gateway from the Amazon CloudWatch console\. For more information about measuring throughput to and from your gateway to AWS, see [Measuring Performance Between Your Gateway and AWS](GatewayMetrics-common.md#PerfGatewayAWS-common)\.  | 

**Warning**  
The elastic IP address of the Amazon EC2 instance cannot be used as the target address\. 

## Enabling AWS Support To Help Troubleshoot Your Gateway Hosted on an Amazon EC2 Instance<a name="EC2-EnableAWSSupportAccess"></a>

AWS Storage Gateway provides a local console you can use to perform several maintenance tasks, including enabling AWS Support to access your gateway to assist you with troubleshooting gateway issues\. By default, AWS Support access to your gateway is disabled\. You enable this access through the Amazon EC2 local console\. You log in to the Amazon EC2 local console through a Secure Shell \(SSH\)\. To successfully log in through SSH, your instance's security group must have a rule that opens TCP port 22\.

**Note**  
If you add a new rule to an existing security group, the new rule applies to all instances that use that security group\. For more information about security groups and how to add a security group rule, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide*\.

To let AWS Support connect to your gateway, you first log in to the local console for the Amazon EC2 instance, navigate to the storage gateway's console, and then provide the access\.

**To enable AWS support access to a gateway deployed on an Amazon EC2 instance**

1. Log in to the local console for your Amazon EC2 instance\. For instructions, go to [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)in the *Amazon EC2 User Guide*\.

   You can use the following command to log in to the EC2 instance's local console\. 

   ```
   ssh –i PRIVATE-KEY sguser@INSTANCE-PUBLIC-DNS-NAME
   ```
**Note**  
The *PRIVATE\-KEY* is the `.pem` file containing the private certificate of the EC2 key pair that you used to launch the Amazon EC2 instance\. For more information, see [Retrieving the Public Key for Your Key Pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#retriving-the-public-key) in the *Amazon EC2 User Guide*\.  
The *INSTANCE\-PUBLIC\-DNS\-NAME* is the public Domain Name System \(DNS\) name of your Amazon EC2 instance that your gateway is running on\. You obtain this public DNS name by selecting the Amazon EC2 instance in the EC2 console and clicking the **Description** tab\.

    The local console looks like the following\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

1. At the prompt, type **3** to open the AWS Storage Gateway console\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGLocalConsole.png)

1. Type **h** to open the **AVAILABLE COMMANDS** window\.

1. In the **AVAILABLE COMMANDS** window, type **open\-support\-channel** to connect to customer support for AWS Storage Gateway\. You must allow TCP port 22 to initiate a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2-assign-service-number.png)
**Note**  
The channel number is not a Transmission Control Protocol/User Datagram Protocol \(TCP/UDP\) port number\. Instead, the gateway makes a Secure Shell \(SSH\) \(TCP 22\) connection to Storage Gateway servers and provides the support channel for the connection\.

1. Once the support channel is established, provide your support service number to AWS Support so AWS Support can provide troubleshooting assistance\. 

1. When the support session is completed, type **q** to end it\.

1. Type **exit** to exit the AWS Storage Gateway console\.

1. Follow the console menus to log out of the AWS Storage Gateway instance\. 