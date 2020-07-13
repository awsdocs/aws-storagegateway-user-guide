# Deploying a Volume or Tape Gateway on an Amazon EC2 Host<a name="ec2-gateway-common"></a>

You can deploy and activate a tape or volume gateway on an Amazon EC2 instance\. The gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**To deploy a gateway on an Amazon EC2 instance**

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a storage gateway EC2 AMI\. You are redirected to the EC2 community AMI page, where you can choose an instance type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/host-ec2-file.png)

1. On the **Choose an Instance Type** page, choose the hardware configuration of your instance\. AWS Storage Gateway is supported on instance types that meet certain minimum requirements\. We recommend starting with the m4xlarge instance type, which meets the minimum requirements for your gateway to function properly\. For more information, see [Hardware requirements for on\-premises VMs](Requirements.md#requirements-hardware)\. 

   You can resize your instance after you launch, if necessary\. For more information, see [Resizing Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
Certain instance types, particularly i3 EC2, use NVMe SSD disks\. These can cause problems when you start or stop a gateway; for example, you can lose data from the cache\. Monitor the `CachePercentDirty` Amazon CloudWatch metric, and only start or stop your system when that metric is `0`\. To learn more about monitoring metrics for your gateway, see [Storage Gateway Metrics and Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/awssg-metricscollected.html) in the CloudWatch documentation\. For more information, see [Requirements for Amazon EC2 instance types](Requirements.md#requirements-hardware-ec2)\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, choose a value for **Auto\-assign Public IP**\. If your instance should be accessible from the public internet, verify that **Auto\-assign Public IP** is set to **Enable**\. If your instance shouldn't be accessible from the internet, choose **Auto\-assign Public IP** for **Disable**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-instance-details.png)

1. On the **Configure Instance Details** page, choose the AWS Identity and Access Management \(IAM\) role that you want to use for your gateway\.

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, choose **Add New Volume** to add storage to your tape or volume gateway instance\. You need at least one Amazon EBS volume to configure for cache storage\.

   The following table recommends sizes for local disk storage for your deployed gateway\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-gateway-common.html)
**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.

1. On the **Step 5: Add Tags** page, you can add an optional tag to your instance\. Then choose **Next: Configure Security Group**\.

1. On the **Configure Security Group** page, add firewall rules to specific traffic to reach your instance\. You can create a new security group or choose an existing security group\. 
**Important**  
Besides the Storage Gateway activation and Secure Shell \(SSH\) access ports, NFS clients require access to additional ports\. For detailed information, see [Network and firewall requirements](Requirements.md#networks)\. 

1. Choose **Review and Launch** to review your configuration\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, and choose the key pair that you created when getting set up\. When you are ready, select the acknowledgment box, and then choose **Launch Instances**\. 

   A confirmation page tells you that your instance is launching\.

1. Choose **View Instances** to close the confirmation page and return to the console\. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is **pending**\. After the instance starts, its state changes to **running**, and it receives a public DNS name\.

1. Choose your instance, note the public IP address in the **Description** tag, and return to the [Connect to gateway](create-gateway-file.md#GettingStartedBeginActivateGateway-file) page on the Storage Gateway console to continue your gateway setup\. 

You can determine the AMI ID to use for launching a tape or volume gateway by using the Storage Gateway console or by querying the AWS Systems Manager parameter store\.

**To determine the AMI ID**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Create gateway**, choose your gateway type, and then choose **Next**\.

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a Storage Gateway EC2 AMI\. You are redirected to the EC2 community AMI page, where you can see the AMI ID for your AWS Region in the URL\.

   Or you can query the Systems Manager parameter store\. You can use the AWS CLI or Storage Gateway API to query the Systems Manager public parameter under the namespace `/aws/service/storagegateway/ami/TAPE/latest`\. For example, using the following CLI command returns the ID of the current AMI in the current AWS Region\.

   ```
   aws --region us-east-2 ssm get-parameter --name /aws/service/storagegateway/ami/TAPE/latest
   ```

   The CLI command returns output similar to the following\.

   ```
   {
       "Parameter": {
           "Type": "String",
           "LastModifiedDate": 1561054105.083,
           "Version": 4,
           "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/storagegateway/ami/TAPE/latest",
           "Name": "/aws/service/storagegateway/ami/TAPE/latest",
           "Value": "ami-123c45dd67d891000"
       }
   }
   ```