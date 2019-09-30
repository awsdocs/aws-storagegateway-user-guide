# Deploying File Gateway on an Amazon EC2 Host<a name="ec2-gateway-file"></a>

You can deploy and activate a file gateway on an Amazon EC2 instance\. The file gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**To deploy a gateway on an Amazon EC2 instance**

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a storage gateway EC2 AMI\. You are redirected to the EC2 community AMI page where you can choose an instance type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/host-ec2-file.png)

1. On the **Choose an Instance Type** page, choose the hardware configuration of your instance\. AWS Storage Gateway is supported on instance types that meet certain minimum requirements\. We recommend starting with the m4.xlarge instance type, which meets the minimum requirements for your gateway to function properly\. For more information, see [Hardware Requirements for On\-Premises VMs](Requirements.md#requirements-hardware)\. 

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

The following shows the file gateway Amazon EC2 AMI names and AMI IDs\.


| Region | AMI Name | AMI ID | 
| --- | --- | --- | 
| ap\-northeast\-1 | aws\-thinstaller\-1560967323 | ami\-0c7b0fb654a4f0f85 | 
| ap\-northeast\-2 | aws\-thinstaller\-1560967323 | ami\-0909f3ce2d2ff17d6 | 
| ap\-south\-1 | aws\-thinstaller\-1560967323 | ami\-04a15305837aaafe7 | 
| ap\-southeast\-1 | aws\-thinstaller\-1560967323 | ami\-06bb5d5822356af8c | 
| ap\-southeast\-2 | aws\-thinstaller\-1560967323 | ami\-069868a07d9275a11 | 
| ca\-central\-1 | aws\-thinstaller\-1560967323 | ami\-02c4655d5f162a934 | 
| eu\-central\-1 | aws\-thinstaller\-1560967323 | ami\-0b899ea99c9087f96 | 
| eu\-north\-1 | aws\-thinstaller\-1560967323 | ami\-0b7548314677edcb4 | 
| eu\-west\-1 | aws\-thinstaller\-1560967323 | ami\-02ea6e7d49330d41b | 
| eu\-west\-2 | aws\-thinstaller\-1560967323 | ami\-0e27eb18911341aa1 | 
| eu\-west\-3 | aws\-thinstaller\-1560967323 | ami\-054a4df140f73927d | 
| sa\-east\-1 | aws\-thinstaller\-1560967323 | ami\-017ad775507f8421d | 
| us\-east\-1 | aws\-thinstaller\-1560967323 | ami\-07b2e2e6e4586930d | 
| us\-east\-2 | aws\-thinstaller\-1560967323 | ami\-0283b5e2cdf0fed9f | 
| us\-west\-1 | aws\-thinstaller\-1560967323 | ami\-0831b9be25c2be35d | 
| us\-west\-2 | aws\-thinstaller\-1560967323 | ami\-0e5a18885401d9460 | 
| us\-gov\-west\-1 | aws\-thinstaller\-1560967323 | ami\-6d6c160c | 
