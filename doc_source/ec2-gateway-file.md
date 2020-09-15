# Deploying a file gateway on an Amazon EC2 host<a name="ec2-gateway-file"></a>

You can deploy and activate a file gateway on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. The file gateway Amazon Machine Image \(AMI\) is available as a community AMI\.

**To deploy a gateway on an Amazon EC2 instance**

1. On the **Select host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a storage gateway EC2 AMI\. You are redirected to the Amazon EC2 console where you can choose an instance type\.

1. On the **Step 2: Choose an Instance Type** page, choose the hardware configuration of your instance\. AWS Storage Gateway is supported on instance types that meet certain minimum requirements\. We recommend starting with the m4\.xlarge instance type, which meets the minimum requirements for your gateway to function properly\. For more information, see [Hardware requirements for on\-premises VMs](Requirements.md#requirements-hardware)\.

   You can resize your instance after you launch, if necessary\. For more information, see [Resizing your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
Certain instance types, particularly i3 EC2, use NVMe SSD disks\. These can cause problems when you start or stop file gateway; for example, you can lose data from the cache\. Monitor the `CachePercentDirty` Amazon CloudWatch metric, and only start or stop your system when that parameter is `0`\. To learn more about monitoring metrics for your gateway, see [Storage Gateway metrics and dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/awssg-metricscollected.html) in the CloudWatch documentation\. For more information about Amazon EC2 instance type requirements, see [Requirements for Amazon EC2 instance types](Requirements.md#requirements-hardware-ec2)\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Step 3: Configure Instance Details** page, choose a value for **Auto\-assign Public IP**\. If your instance should be accessible from the public internet, verify that **Auto\-assign Public IP** is set to **Enable**\. If your instance shouldn't be accessible from the internet, choose **Auto\-assign Public IP** for **Disable**\.

1. For **IAM role**, choose the AWS Identity and Access Management \(IAM\) role that you want to use for your gateway\.

1. Choose **Next: Add Storage**\.

1. On the **Step 4: Add Storage** page, choose **Add New Volume** to add a second volume to use as a cache for your file gateway instance\. In addition to your boot volume, you need at least one additional Amazon EBS volume to configure for cache storage\.

   The following table recommends sizes for local disk storage for your deployed gateway\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-gateway-file.html)
**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.

1. On the **Step 5: Add Tags** page, you can add an optional tag to your instance\. Then choose **Next: Configure Security Group**\.

1. On the **Step 6: Configure Security Group** page, add firewall rules to specific traffic to reach your instance\. You can create a new security group or choose an existing security group\.
**Important**  
Besides the Storage Gateway activation and Secure Shell \(SSH\) access ports, NFS clients require access to additional ports\. For detailed information, see [Network and firewall requirements](Requirements.md#networks)\.

1. Choose **Review and Launch** to review your configuration\.

1. On the **Step 7: Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, and then select the key pair that you created when getting set up\. When you are ready, choose the acknowledgment box, and then choose **Launch Instances**\.

   A confirmation page tells you that your instance is launching\.

1.  Choose **View Instances** to close the confirmation page and return to the console\. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is **pending**\. After the instance starts, its state changes to **running**, and it receives a public DNS name

1. Select your instance, note the public IP address in the **Description** tag, and return to the [Connect to gateway](create-gateway-file.md#GettingStartedBeginActivateGateway-file) page on the Storage Gateway console to continue your gateway setup\.

You can determine the AMI ID to use for launching a file gateway by using the Storage Gateway console or by querying the AWS Systems Manager parameter store\.

**To determine the AMI ID**

1. Sign in to the AWS Management Console and open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Create gateway**, choose **File gateway**, and then choose **Next**\.

1. On the **Choose host platform** page, choose **Amazon EC2**\.

1. Choose **Launch instance** to launch a Storage Gateway EC2 AMI\. You are redirected to the EC2 community AMI page, where you can see the AMI ID for your AWS Region in the URL\.

   Or you can query the Systems Manager parameter store\. You can use the AWS CLI or Storage Gateway API to query the Systems Manager public parameter under the namespace `/aws/service/storagegateway/ami/FILE_S3/latest`\. For example, using the following CLI command returns the ID of the current AMI in the current AWS Region\.

   ```
   aws --region us-east-2 ssm get-parameter --name /aws/service/storagegateway/ami/FILE_S3/latest
   ```

   The CLI command returns output similar to the following\.

   ```
   {
       "Parameter": {
           "Type": "String",
           "LastModifiedDate": 1561054105.083,
           "Version": 4,
           "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/storagegateway/ami/FILE_S3/latest",
           "Name": "/aws/service/storagegateway/ami/FILE_S3/latest",
           "Value": "ami-123c45dd67d891000"
       }
   }
   ```
