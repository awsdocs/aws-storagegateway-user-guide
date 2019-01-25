# Deploying File Gateway on an Amazon EC2 Host<a name="ec2-gateway-file"></a>

You can deploy and activate a file gateway on an Amazon EC2 instance\. The file gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**To deploy a gateway on an Amazon EC2 instance**

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a storage gateway EC2 AMI\. You are redirected to the EC2 community AMI page where you can choose an instance type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/host-ec2-file.png)

1. On the **Choose an Instance Type** page, choose the hardware configuration of your instance\. AWS Storage Gateway is supported on instance types that meet certain minimum requirements\. We recommend starting with the m4xlarge instance type, which meets the minimum requirements for your gateway to function properly\. For more information, see [Hardware Requirements for On\-Premises VMs](Requirements.md#requirements-hardware)\. 

   You can resize your instance after you launch, if necessary\. For more information, see [Resizing Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
Certain instance types, particularly i3 EC2, use NVMe SSD disk\. These can cause problems when you start/stop File Gateway you can lose data from the cache\. Monitor the `CachePercentDirty` Amazon CloudWatch metric, and only start/stop your system when that paramater is `0`\. To learn more about monitoring metrics for your gateway, go to [storage gateway metrics and dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/awssg-metricscollected.html)\. Refer to [Requirements for Amazon EC2 Instance Types](Requirements.md#requirements-hardware-ec2) for more information on this topic\.

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
| ap\-northeast\-1 | aws\-thinstaller\-1528922603 | ami\-0f71c6da06fb8649d | [Launch instance](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0f71c6da06fb8649d) | 
| ap\-northeast\-2 | aws\-thinstaller\-1528922603 | ami\-03bacd44ea49c26b0 | [Launch instance](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-03bacd44ea49c26b0) | 
| ap\-south\-1 | aws\-thinstaller\-1528922603 | ami\-082eb7dd6f3ebd317 | [Launch instance](https://ap-south-1.console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-082eb7dd6f3ebd317) | 
| ap\-southeast\-1 | aws\-thinstaller\-1528922603 | ami\-0aa88018a7a8a9c8c | [Launch instance](https://ap-southeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0aa88018a7a8a9c8c) | 
| ap\-southeast\-2 | aws\-thinstaller\-1528922603 | ami\-0ca4c65c606d56fd7 | [Launch instance](https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0ca4c65c606d56fd7) | 
| ca\-central\-1 | aws\-thinstaller\-1528922603 | ami\-06289126fa6074fcc | [Launch instance](https://ca-central-1.console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-06289126fa6074fcc) | 
| eu\-central\-1 | aws\-thinstaller\-1528922603 | ami\-0f001eb2d3ca82aa9 | [Launch instance](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami-0f001eb2d3ca82aa9) | 
| eu\-north\-1 | aws\-thinstaller\-1542045415 | ami\-020897557c719806a | [Launch instance](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami-020897557c719806a) | 
| eu\-west\-1 | aws\-thinstaller\-1528922603 | ami\-08a8a9ac2f4bdaab3 | [Launch instance](https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-08a8a9ac2f4bdaab3) | 
| eu\-west\-2 | aws\-thinstaller\-1528922603 | ami\-03eb1b6c734250902 | [Launch instance](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-03eb1b6c734250902) | 
| eu\-west\-3 | aws\-thinstaller\-1528922603 | ami\-004ff65e137137207 | [Launch instance](https://eu-west-2.console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-004ff65e137137207) | 
| sa\-east\-1 | aws\-thinstaller\-1528922603 | ami\-088a94719b5d1c8bc | [Launch instance](https://sa-east-1.console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-088a94719b5d1c8bc) | 
| us\-east\-1 | aws\-thinstaller\-1528922603 | ami\-0eabc4464d7df5719 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0eabc4464d7df5719) | 
| us\-east\-2 | aws\-thinstaller\-1528922603 | ami\-02ae2bd0799b31744 | [Launch instance](https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-02ae2bd0799b31744) | 
| us\-west\-1 | aws\-thinstaller\-1528922603 | ami\-0dd8d0c739942501e | [Launch instance](https://us-west-1.console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0dd8d0c739942501e) | 
| us\-west\-2 | aws\-thinstaller\-1528922603 | ami\-0077f2b393ecfacb3 | [Launch instance](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0077f2b393ecfacb3) | 