--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Creating a Gateway<a name="create-volume-gateway"></a>

In this section, you can find instructions on how to download, deploy, and activate a Volume Gateway\. 

**Topics**
+ [Set up a Volume Gateway](#set-up-gateway-volume)
+ [Connect your Volume Gateway to AWS](#connect-to-amazon-volume)
+ [Review settings and activate your Volume Gateway](#review-and-activate-volume)
+ [Configure your Volume Gateway](#configure-gateway-volume)

## Set up a Volume Gateway<a name="set-up-gateway-volume"></a>

**To set up a new Volume Gateway**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/storagegateway/home/](https://console.aws.amazon.com/storagegateway/home/), and choose the AWS Region where you want to create your gateway\.

1. Choose **Create gateway** to open the **Set up gateway** page\.

1. In the **Gateway settings** section, do the following:

   1. For **Gateway name**, enter a name for your gateway\. You can search for this name to find your gateway on list pages in the Storage Gateway console\.

   1. For **Gateway time zone**, choose the local time zone for the part of the world where you want to deploy your gateway\.

1. In the **Gateway options** section, for **Gateway type**, choose **Volume Gateway**, then choose the volume type your gateway will use\. You can choose from the following options:
   + **Cached volumes** \- Stores your primary data in Amazon S3 and retains frequently accessed data locally in cache for faster access\.
   + **Stored volumes** \- Stores all of your data locally while also backing it up asynchronously to Amazon S3\. Gateways using this volume type cannot be deployed on Amazon EC2\.

1. In the **Platform options** section, do the following:

   1. For **Host platform**, choose the platform on which you want to deploy your gateway, then follow the platform\-specific instructions displayed on the Storage Gateway console page to set up your host platform\. You can choose from the following options:
      + **VMware ESXi** \- Download, deploy, and configure the gateway virtual machine using VMware ESXi\.
      + **Microsoft Hyper\-V** \- Download, deploy, and configure the gateway virtual machine using Microsoft Hyper\-V\.
      + **Linux KVM** \- Download, deploy, and configure the gateway virtual machine using Linux KVM\.
      + **Amazon EC2** \- Configure and launch an Amazon EC2 instance to host your gateway\. This option is not available for **Stored volume** gateways\.
      + **Hardware appliance** \- Order a dedicated physical hardware appliance from AWS to host your gateway\.

   1. For **Confirm set up gateway**, select the checkbox to confirm that you performed the deployment steps for the host platform you chose\. This step is not applicable for the **Hardware appliance** host platform\.

1. Choose **Next** to proceed\.

Now that your gateway is set up, you need to choose how you want it to connect and communicate with AWS\. For instructions, see [Connect your Volume Gateway to AWS](https://docs.aws.amazon.com/storagegateway/latest/userguide/connect-to-amazon-volume.html)\.

## Connect your Volume Gateway to AWS<a name="connect-to-amazon-volume"></a>

**To connect a new Volume Gateway to AWS**

1. Complete the procedure described in [Set up a Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/set-up-gateway-volume.html) if you have not done so already\. When finished, choose **Next** to open the **Connect to AWS** page in the Storage Gateway console\.

1. In the **Endpoint options** section, for **Service endpoint**, choose the type of endpoint your gateway will use to communicate with AWS\. You can choose from the following options:
   + **Publicly accessible** \- Your gateway communicates with AWS over the public internet\. If you select this option, use the **FIPS enabled endpoint** checkbox to specify whether the connection should comply with Federal Information Processing Standards \(FIPS\)\.
**Note**  
If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS\-compliant endpoint\. For more information, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.  
The FIPS service endpoint is only available in some AWS Regions\. For more information, see [Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.
   + **VPC hosted** \- Your gateway communicates with AWS through a private connection with your VPC, allowing you to control your network settings\. If you select this option, you must specify an existing VPC endpoint by choosing its VPC endpoint ID from the drop\-down menu, or by providing its VPC endpoint DNS name or IP address\.

1. In the **Gateway connection options** section, for **Connection options**, choose how to identify your gateway to AWS\. You can choose from the following options:
   + **IP address** \- Provide the IP address of your gateway in the corresponding field\. This IP address must be public or accessible from within your current network, and you must be able to connect to it from your web browser\.

     You can obtain the gateway IP address by logging into the gateway's local console from your hypervisor client, or by copying it from your Amazon EC2 instance details page\.
   + **Activation key** \- Provide the activation key for your gateway in the corresponding field\. You can generate an activation key using the gateway's local console\. Choose this option if your gateway's IP address is unavailable\.

1. Choose **Next** to proceed\.

Now that you have chosen how you want your gateway to connect to AWS, you need to activate the gateway\. For instructions, see [Review settings and activate your Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/review-and-activate-volume.html)\.

## Review settings and activate your Volume Gateway<a name="review-and-activate-volume"></a>

**To activate a new Volume Gateway**

1. Complete the procedures described in the following topics if you have not done so already:
   + [Set up a Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/set-up-gateway-volume.html)
   + [Connect your Volume Gateway to AWS](https://docs.aws.amazon.com/storagegateway/latest/userguide/connect-to-amazon-volume.html)

   When finished, choose **Next** to open the **Review and activate** page in the Storage Gateway console\.

1. Review the initial gateway details for each section on the page\.

1. If a section contains errors, choose **Edit** to return to the corresponding settings page and make changes\.
**Note**  
You cannot modify the gateway options or connection settings after your gateway is created\.

1. Choose **Next** to proceed\.

Now that you have activated your gateway, you need to perform first\-time configuration to allocate local storage disks and configure logging\. For instructions, see [Configure your Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/configure-gateway-volume.html)\.

## Configure your Volume Gateway<a name="configure-gateway-volume"></a>

**To perform first\-time configuration on a new Volume Gateway**

1. Complete the procedures described in the following topics if you have not done so already:
   + [Set up a Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/set-up-gateway-volume.html)
   + [Connect your Volume Gateway to AWS](https://docs.aws.amazon.com/storagegateway/latest/userguide/connect-to-amazon-volume.html)
   + [Review settings and activate your Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/review-and-activate-volume.html)

   When finished, choose **Next** to open the **Configure gateway** page in the Storage Gateway console\.

1. In the **Configure cache storage and upload buffer** section, use the drop\-down menus to allocate at least one local disk with at least 150 GiB capacity to both **Cache** and **Upload buffer**\. The local disks listed in this section correspond to the physical storage that you provisioned on your host platform\.

1. In the **CloudWatch log group** section, choose how to set up Amazon CloudWatch Logs to monitor the health of your gateway\. You can choose from the following options:
   + **Create a new log group** \- Set up a new log group to monitor your gateway\.
   + **Use an existing log group** \- Choose an existing log group from the corresponding drop\-down menu\.
   + **Deactivate logging** \- Do not use Amazon CloudWatch Logs to monitor your gateway\.

1. In the **CloudWatch alarms** section, choose how to set up Amazon CloudWatch alarms to notify you when gateway metrics deviate from defined limits\. You can choose from the following options:
   + **Deactivate alarms** \- Do not use CloudWatch alarms to be notified about gateway metrics\.
   + **Create custom CloudWatch alarm** \- Configure a new CloudWatch alarm to be notified about gateway metrics\. Choose Create alarm to define metrics and specify alarm actions in the CloudWatch console\. For instructions, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\.

1. \(Optional\) In the **Tags** section, choose **Add new tag**, then enter a case\-sensitive key\-value pair to help you search and filter for your gateway on list pages in the Storage Gateway console\. Repeat this step to add as many tags as you need\.

1. \(Optional\) In the **Verify VMware High Availability configuration** section, if your gateway is deployed on a VMware host as part of a cluster that is enabled for VMware High Availability \(HA\), choose **Verify VMware HA** to test whether the HA configuration is working properly\.
**Note**  
This section appears only for gateways running on the VMware host platform\.  
This step is not required to complete the gateway configuration process\. You can test your gateway's HA configuration at any time\. Verification takes a few minutes, and reboots the Storage Gateway VM\.

1. Choose **Configure** to finish creating your gateway\.

   To check the status of your new gateway, search for it on the **Gateways** page of the Storage Gateway\.

Now that you have created your gateway, you need to create a volume for it to use\. For instructions, see [Creating a volume](https://docs.aws.amazon.com/storagegateway/latest/userguide/GettingStartedCreateVolumes.html)\.