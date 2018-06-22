# Deploying File Gateway on an Amazon EC2 Host<a name="ec2-gateway-file"></a>

You can deploy and activate a file gateway on an Amazon EC2 instance\. The file gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**To deploy a gateway on an Amazon EC2 instance**

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a storage gateway EC2 AMI\. You are redirected to the EC2 community AMI page where you can choose an instance type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/host-ec2-file.png)

1. On the **Choose an Instance Type** page, choose the hardware configuration of your instance\. AWS Storage Gateway is supported on instance types that meet certain minimum requirements\. We recommend starting with the m4xlarge instance type, which meets the minimum requirements for your gateway to function properly\. For more information, see [Hardware Requirements](Requirements.md#requirements-hardware)\. 

   You can resize your instance after you launch, if necessary\. For more information, see [Resizing Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, choose a value for **Auto\-assign Public IP**\. If your instance should be accessible from the public Internet, verify that **Auto\-assign Public IP** is set to **Enable**\. If your instance should not be accessible from the Internet, choose **Auto\-assign Public IP** for **Disable**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-instance-details.png)

1. On the **Configure Instance Details** page, choose the AWS Identity and Access Management \(IAM\) role that you want to use for your gateway\.

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, choose **Add New Volume** to add storage to your file gateway instance\. You need at least one Amazon EBS volume to configure for cache storage\.

   The following table recommends sizes for local disk storage for your deployed gateway\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-gateway-file.html)
**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.

1. On the **Step 5: Add Tags** page, you can add an optional tag to your instance\. Then choose **Next: Configure Security Group**\.

1. On the **Configure Security Group** page, add firewall rules to specific traffic to reach our instance\. You can create a new security group or choose an existing security group\. 
**Important**  
Besides the Storage Gateway activation and Secure Shell \(SSH\) access ports, NFS clients require access to additional ports\. For detailed information, see [Network and Firewall Requirements](Requirements.md#networks)\. 

1. Choose **Review and Launch** to review your configuration\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, and then select the key pair that you created when getting set up\. When you are ready, choose the acknowledgment box, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is **pending**\. After the instance starts, its state changes to **running**, and it receives a public DNS name

1. Select your instance, take note of the public IP address in the **Description** tag and return to the [Connect to gateway](create-gateway-file.md#GettingStartedBeginActivateGateway-file) page on the Storage Gateway console to continue your gateway setup\.

The following table lists the available Storage Gateway AMIs by region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| ap\-northeast\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-946d2cf2 | [Launch instance](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-946d2cf2) | 
| ap\-northeast\-2 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-5974d937 | [Launch instance](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-5974d937) | 
| ap\-south\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-4e9fc021 | [Launch instance](https://ap-south-1.console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-4e9fc021) | 
| ap\-southeast\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-ae94dfd2 | [Launch instance](https://ap-southeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-ae94dfd2) | 
| ap\-southeast\-2 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-7118de13 | [Launch instance](https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-7118de13) | 
| ca\-central\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-17961173 | [Launch instance](https://ca-central-1.console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-17961173) | 
| eu\-central\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-010f636e | [Launch instance](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami-010f636e) | 
| eu\-west\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-e19dd998 | [Launch instance](https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-e2de5b9b) | 
| eu\-west\-2 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-07779360 | [Launch instance](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-07779360) | 
| eu\-west\-3 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-3e79cf43 | [Launch instance](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-3e79cf43) | 
| sa\-east\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-a34a01cf | [Launch instance](https://sa-east-1.console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-a34a01cf) | 
| us\-east\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-edd92c90 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-edd92c90) | 
| us\-east\-2 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-1ce9de79 | [Launch instance](https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-1ce9de79) | 
| us\-west\-1 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-5a969d3a | [Launch instance](https://us-west-1.console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-5a969d3a) | 
| us\-west\-2 | aws\-storage\-gateway\-file\-2018\-03\-01 | ami\-5c3fb724 | [Launch instance](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-5c3fb724) | 