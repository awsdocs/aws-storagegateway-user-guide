# What is Amazon S3 File Gateway<a name="what-is-file-s3"></a>

AWS Storage Gateway connects an on\-premises software appliance with cloud\-based storage to provide seamless integration with data security features between your on\-premises IT environment and the AWS storage infrastructure\. You can use the service to store data in the AWS Cloud for scalable and cost\-effective storage that helps maintain data security\. AWS Storage Gateway offers file\-based, volume\-based, and tape\-based storage solutions\.

**Topics**
+ [Amazon S3 File Gateway](#gateway-file-s3)

## Amazon S3 File Gateway<a name="gateway-file-s3"></a>

**Amazon S3 File Gateway** –Amazon S3 File Gateway supports a file interface into [Amazon Simple Storage Service \(Amazon S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) and combines a service and a virtual software appliance\. By using this combination, you can store and retrieve objects in Amazon S3 using industry\-standard file protocols such as Network File System \(NFS\) and Server Message Block \(SMB\)\. The software appliance, or gateway, is deployed into your on\-premises environment as a virtual machine \(VM\) running on VMware ESXi, Microsoft Hyper\-V, or Linux Kernel\-based Virtual Machine \(KVM\) hypervisor\. The gateway provides access to objects in S3 as files or file share mount points\. With a S3 File, you can do the following:
+ You can store and retrieve files directly using the NFS version 3 or 4\.1 protocol\.
+ You can store and retrieve files directly using the SMB file system version, 2 and 3 protocol\.
+ You can access your data directly in Amazon S3 from any AWS Cloud application or service\.
+ You can manage your S3 data using lifecycle policies, cross\-region replication, and versioning\. You can think of a S3 File as a file system mount on Amazon S3\.

A S3 File simplifies file storage in Amazon S3, integrates to existing applications through industry\-standard file system protocols, and provides a cost\-effective alternative to on\-premises storage\. It also provides low\-latency access to data through transparent local caching\. A S3 File manages data transfer to and from AWS, buffers applications from network congestion, optimizes and streams data in parallel, and manages bandwidth consumption\. S3 File integrate with AWS services, for example with the following:
+ Common access management using AWS Identity and Access Management \(IAM\)
+ Encryption using AWS Key Management Service \(AWS KMS\)
+ Monitoring using Amazon CloudWatch \(CloudWatch\)
+ Audit using AWS CloudTrail \(CloudTrail\)
+ Operations using the AWS Management Console and AWS Command Line Interface \(AWS CLI\)
+ Billing and cost management

In the following documentation, you can find a Getting Started section that covers setup information common to all gateways and also gateway\-specific setup sections\. The Getting Started section shows you how to deploy, activate, and configure storage for a gateway\. The management section shows you how to manage your gateway and resources:
+  provides instructions on how to create and use a S3 File\. It shows you how to create a file share, map your drive to an Amazon S3 bucket, and upload files and folders to Amazon S3\.
+  describes how to perform management tasks for all gateway types and resources\.

In this guide, you can primarily find how to work with gateway operations by using the AWS Management Console\. If you want to perform these operations programmatically, see the *[AWS Storage Gateway API Reference](https://docs.aws.amazon.com/storagegateway/latest/APIReference/)\.*