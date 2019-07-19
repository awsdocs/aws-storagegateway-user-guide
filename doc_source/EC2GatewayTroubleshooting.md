# Troubleshooting Amazon EC2 Gateway Issues<a name="EC2GatewayTroubleshooting"></a>

In the following sections, you can find typical issues that you might encounter working with your gateway deployed on Amazon EC2\. For more information about the difference between an on\-premises gateway and a gateway deployed in Amazon EC2, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. 

**Topics**
+ [Your Gateway Activation Hasn't Occurred After a Few Moments](#activation-issues)
+ [You Can't Find Your EC2 Gateway Instance in the Instance List](#find-instance)
+ [You Created an Amazon EBS Volume But Can't Attach it to Your EC2 Gateway Instance](#ebs-volume-issue)
+ [You Can't Attach an Initiator to a Volume Target of Your EC2 Gateway](#initiator-issue)
+ [You Get a Message That You Have No Disks Available When You Try to Add Storage Volumes](#no-disk)
+ [You Want to Remove a Disk Allocated as Upload Buffer Space to Reduce Upload Buffer Space](#uploadbuffer-issue)
+ [Throughput to or from Your EC2 Gateway Drops to Zero](#gateway-throughput-issue)
+ [You Want Your File Gateway to Use a C5 or M5 EC2 Instance Type Instead of C4 or M4](#ami-upgrade)
+ [You Want AWS Support to Help Troubleshoot Your EC2 Gateway](#EC2-EnableAWSSupportAccess)

## Your Gateway Activation Hasn't Occurred After a Few Moments<a name="activation-issues"></a>

Check the following in the Amazon EC2 console:
+ Port 80 is enabled in the security group you associated with the instance\. For more information about adding a security group rule, see [Adding a Security Group Rule](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule) in the *Amazon EC2 User Guide for Linux Instances*\.
+ The gateway instance is marked as running\. In the Amazon EC2 console, the **State** value for the instance should be RUNNING\.
+ Make sure that your Amazon EC2 instance type meets the minimum requirements, as described in [Storage Requirements](Requirements.md#requirements-storage)\.

After correcting the problem, try activating the gateway again by going to the AWS Storage Gateway console, choosing **Deploy a new Gateway on Amazon EC2**, and re\-entering the IP address of the instance\.

## You Can't Find Your EC2 Gateway Instance in the Instance List<a name="find-instance"></a>

If you didn't give your instance a resource tag and you have many instances running, it can be hard to tell which instance you launched\. In this case, you can take the following actions to find the gateway instance:
+ Check the name of the Amazon Machine Image \(AMI\) on the **Description** tab of the instance\. An instance based on the AWS Storage Gateway AMI should start with the text **aws\-storage\-gateway\-ami**\.
+ If you have several instances based on the AWS Storage Gateway AMI, check the instance launch time to find the correct instance\. 

## You Created an Amazon EBS Volume But Can't Attach it to Your EC2 Gateway Instance<a name="ebs-volume-issue"></a>

Check that the Amazon EBS volume in question is in the same Availability Zone as the gateway instance\. If there is a discrepancy in Availability Zones, create a new Amazon EBS volume in the same Availability Zone as your instance\.

## You Can't Attach an Initiator to a Volume Target of Your EC2 Gateway<a name="initiator-issue"></a>

Check that the security group that you launched the instance with includes a rule that allows the port that you are using for iSCSI access\. The port is usually set as 3260\. For more information on connecting to volumes, see [Connecting to Your Volumes to a Windows Client](initiator-connection-common.md#ConfiguringiSCSIClient)\.

## You Get a Message That You Have No Disks Available When You Try to Add Storage Volumes<a name="no-disk"></a>

For a newly activated gateway, no volume storage is defined\. Before you can define volume storage, you must allocate local disks to the gateway to use as an upload buffer and cache storage\. For a gateway deployed to Amazon EC2, the local disks are Amazon EBS volumes attached to the instance\. This error message likely occurs because no Amazon EBS volumes are defined for the instance\. 

Check block devices defined for the instance that is running the gateway\. If there are only two block devices \(the default devices that come with the AMI\), then you should add storage\. For more information on doing so, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\. After attaching two or more Amazon EBS volumes, try creating volume storage on the gateway\.

## You Want to Remove a Disk Allocated as Upload Buffer Space to Reduce Upload Buffer Space<a name="uploadbuffer-issue"></a>

Follow the steps in [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.

## Throughput to or from Your EC2 Gateway Drops to Zero<a name="gateway-throughput-issue"></a>

Verify that the gateway instance is running\. If the instance is starting due to a reboot, for example, wait for the instance to restart\.

Also, verify that the gateway IP has not changed\. If the instance was stopped and then restarted, the IP address of the instance might have changed\. In this case, you need to activate a new gateway\.

You can view the throughput to and from your gateway from the Amazon CloudWatch console\. For more information about measuring throughput to and from your gateway to AWS, see [Measuring Performance Between Your Gateway and AWS](GatewayMetrics-common.md#PerfGatewayAWS-common)\.

## You Want Your File Gateway to Use a C5 or M5 EC2 Instance Type Instead of C4 or M4<a name="ami-upgrade"></a>

Do the following:

1. Create a new file gateway using the c5 or m5 Amazon EC2 AMI\.

1. Create a new file share on the new gateway and configure it to point to your Amazon S3 bucket\.

1. Mount your new file share to your client\.

1. Make sure that your file gateway that is using a c4 or m4 EC2 AMI has finished uploading all data to S3 \(that is, the `CachePercentDirty` value is 0\)\.

1. Shut down the file gateway that is using a c4 or m4 AMI and delete the gateway if you no longer need it\.

For information about instance type requirements, see [Hardware and Storage Requirements](Requirements.md#requirements-hardware-storage)\.

**Warning**  
You can't use the elastic IP address of the Amazon EC2 instance used as the target address\. 

## You Want AWS Support to Help Troubleshoot Your EC2 Gateway<a name="EC2-EnableAWSSupportAccess"></a>

AWS Storage Gateway provides a local console you can use to perform several maintenance tasks, including enabling AWS Support to access your gateway to assist you with troubleshooting gateway issues\. By default, AWS Support access to your gateway is disabled\. You enable this access through the Amazon EC2 local console\. You log in to the Amazon EC2 local console through a Secure Shell \(SSH\)\. To successfully log in through SSH, your instance's security group must have a rule that opens TCP port 22\.

**Note**  
If you add a new rule to an existing security group, the new rule applies to all instances that use that security group\. For more information about security groups and how to add a security group rule, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide*\.

To let AWS Support connect to your gateway, you first log in to the local console for the Amazon EC2 instance, navigate to the storage gateway's console, and then provide the access\.

**To enable AWS support access to a gateway deployed on an Amazon EC2 instance**

1. Log in to the local console for your Amazon EC2 instance\. For instructions, go to [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)in the *Amazon EC2 User Guide*\.

   You can use the following command to log in to the EC2 instance's local console\. 

   ```
   ssh –i PRIVATE-KEY admin@INSTANCE-PUBLIC-DNS-NAME
   ```
**Note**  
The *PRIVATE\-KEY* is the `.pem` file containing the private certificate of the EC2 key pair that you used to launch the Amazon EC2 instance\. For more information, see [Retrieving the Public Key for Your Key Pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#retriving-the-public-key) in the *Amazon EC2 User Guide*\.  
The *INSTANCE\-PUBLIC\-DNS\-NAME* is the public Domain Name System \(DNS\) name of your Amazon EC2 instance that your gateway is running on\. You obtain this public DNS name by selecting the Amazon EC2 instance in the EC2 console and clicking the **Description** tab\.

    The local console looks like the following\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

1. At the prompt, type **3** to open the AWS Storage Gateway console\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGLocalConsole.png)

1. Type **h** to open the **AVAILABLE COMMANDS** window\.

1. 
   + If you gateway is using a public endpoint, in the **AVAILABLE COMMANDS** window, type **open\-support\-channel** to connect to customer support for AWS Storage Gateway\. You must allow TCP port 22 to initiate a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.
   + If you gateway is using a VPC endpoint, in the **AVAILABLE COMMANDS** window, type **open\-support\-channel**, If your gateway is not activated, provide the *VPC endpoint or IP address* to connect to customer support for AWS Storage Gateway\. You must allow TCP port 22 to initiate a support channel to AWS\. When you connect to customer support, Storage Gateway assigns you a support number\. Make a note of your support number\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2-assign-service-number.png)
**Note**  
The channel number is not a Transmission Control Protocol/User Datagram Protocol \(TCP/UDP\) port number\. Instead, the gateway makes a Secure Shell \(SSH\) \(TCP 22\) connection to Storage Gateway servers and provides the support channel for the connection\.

1. Once the support channel is established, provide your support service number to AWS Support so AWS Support can provide troubleshooting assistance\. 

1. When the support session is completed, type **q** to end it\.

1. Type **exit** to exit the AWS Storage Gateway console\.

1. Follow the console menus to log out of the AWS Storage Gateway instance\. 