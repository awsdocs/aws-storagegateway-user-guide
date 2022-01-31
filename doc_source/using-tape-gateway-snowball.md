--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Using a Tape Gateway on an AWS Snowball Edge device<a name="using-tape-gateway-snowball"></a>

Using a Tape Gateway on an AWS Snowball Edge device provides a secure, offline solution for migrating your tape data\. With a Tape Gateway on a Snowball Edge device, you can migrate petabytes of data stored on physical tapes to AWS without changing your existing tape\-based backup workflows, and without extreme network infrastructure or bandwidth\-usage requirements\. A standard Tape Gateway temporarily stores your tape data in the gateway cache and uses your network connection to transfer the data asynchronously to the AWS Cloud\. However, a Tape Gateway on a Snowball Edge device stores your tape data on the device itself until you return it to AWS, and uses your network connection only for device\-management traffic\.

An AWS Snowball Edge device is one of the Snow Family Devices—a physical hardware device, shipped to you from AWS, which helps you transfer data into and out of the AWS Cloud\. After you receive the device, you unlock it, set up a Tape Gateway on it, copy your tape data to it, and then ship it back to AWS\. AWS then stores your tape data in secure, durable, and low\-cost Amazon S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage\. With this combination of technologies, you can migrate your tape data to AWS in situations where you have network\-connectivity limitations, bandwidth constraints, or high connection costs\. Moving tape data to AWS helps you decrease your physical\-tape infrastructure expenses and gain online access to your tape\-based data at any time\.

The following sections provide detailed instructions on creating, managing, using, and troubleshooting a Tape Gateway on a Snowball Edge device\. For more information about ordering and deploying your Snowball Edge device with a Tape Gateway, see [Using an AWS Snowball Edge Device with a Tape Gateway](https://docs.aws.amazon.com/snowball/latest/developer-guide/using-snowball-tape-gateway.html) in the *AWS Snowball Edge Developer Guide*\.

## Creating a Tape Gateway on your Snowball Edge device<a name="creating-tape-gateway-snowball"></a>

Before you can create a Tape Gateway on a Snowball Edge device, you must order and deploy a Snowball Edge device that's preconfigured for use with Tape Gateway, and then launch the Tape Gateway application service on it\. For instructions, see [Using an AWS Snowball Edge Device with a Tape Gateway](https://docs.aws.amazon.com/snowball/latest/developer-guide/using-snowball-tape-gateway.html) in the *AWS Snowball Edge Developer Guide*\.

After you launch the Tape Gateway application service on your Snowball Edge device, use the following procedure to create and activate your Tape Gateway using the AWS Storage Gateway console\.

**Note**  
Make sure that your network environment meets the network and firewall requirements for Tape Gateway\. For more information, see [Network and firewall requirements](Requirements.md#networks)\.

**To create a gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/home), select the Region where you ordered your Snowball Edge device, and then choose **Create gateway**\.
**Important**  
You must create and activate the Tape Gateway in the same Region where you ordered the Snowball Edge device\.
**Note**  
You can also open the AWS Storage Gateway console directly from within the AWS OpsHub for Snow Family Snow Family management software\. To do so, choose **Open Storage Gateway management console** from the AWS OpsHub management dashboard on your Snowball Edge device\.

1. For **Gateway name**, enter a name for your gateway\.

1. For **Gateway time zone**, choose the local time zone for your gateway\.

1. For **Gateway type**, choose **Tape Gateway**\.

1. For **Host platform**, choose **Snowball Edge**

1. For **Confirm Snowball Edge configuration**, select the checkbox to confirm that you have ordered and deployed your Snowball Edge device\.

1. For **Backup application**, choose the application you want to use to copy your tape data\.

1. Choose **Next**\.

1. For **Service endpoint**, choose **Publicly accessible**\.
**Note**  
**Publicly accessible** is the only service endpoint type that supports a Tape Gateway on a Snowball Edge device\.

1. For **Connection options**, choose **IP address** and enter the Tape Gateway IP address that you obtained when you launched the Tape Gateway application service on your Snowball Edge device\.

   For instructions on how to obtain the Tape Gateway IP address, see [Deploying a Snowball Edge Device with a Tape Gateway](https://docs.aws.amazon.com/snowball/latest/developer-guide/using-snowball-tape-gateway.html#deploying-snowball-tape-gateway) in the *AWS Snowball Edge Developer Guide*\.

1. Choose **Next**\.

1. On the **Review and create** page, make sure that the options you selected are correct, then choose **Create gateway**\.

1. For **CloudWatch log group**, configure Amazon CloudWatch logging\. For more information, see [Getting Tape Gateway Health Logs with CloudWatch Log Groups](GatewayMetrics-vtl-common.md#cw-log-groups-tape)\.

1. \(Optional\) Add tags to identify the gateway\.

1. Choose **Configure** to finish configuring your Tape Gateway\.

   You can check the status of your new gateway on the **Gateways** page of the Storage Gateway console, or select its name to view additional details\. For a Tape Gateway on a Snowball Edge device, the **Gateway type** is **Tape \(Snow\)**, and the **Host platform type** is **Snowball Edge**\.

Now that you have created and activated your Tape Gateway on your Snowball Edge device, you must create the virtual tapes that the gateway uses to back up your tape data\. For instructions, see [Creating tapes for a Tape Gateway on a Snowball Edge device](#creating-tapes-snowball)\.

## Creating tapes for a Tape Gateway on a Snowball Edge device<a name="creating-tapes-snowball"></a>

After creating and activating your Tape Gateway on your Snowball Edge device, use the following procedure to create the virtual tapes that the gateway uses to back up your tape data\. For more information about configuring virtual tapes, see [Creating Tapes](GettingStartedCreateTapes.md)\.

**To create virtual tapes**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/home)\.

1. In the left navigation pane, choose **Tapes** from the navigation pane, and then choose **Create tapes**\.

1. For **Select gateway**, choose the Tape Gateway that you created on your Snowball Edge device\.

1. For **Tape configuration**, specify the type, quantity, capacity, barcode prefix, and pool for the tapes that you want to create\.

1. \(Optional\) For **Tags**, add key\-value pairs to describe your tapes\.

1. Choose **Create tapes**\.

Now that your tapes are set up, your gateway is ready to use\. Before you start backing up your tape data, see [Troubleshooting and best practices for a Tape Gateway on a Snowball Edge device](#troubleshooting-best-practices-tape-gateway-snowball) for tips and guidelines to avoid problems and keep your gateway running smoothly\.

## Troubleshooting and best practices for a Tape Gateway on a Snowball Edge device<a name="troubleshooting-best-practices-tape-gateway-snowball"></a>

To avoid problems and keep your Tape Gateway on Snowball Edge running smoothly, refer to the following tips and guidelines:
+ After you order, receive, and deploy your Snowball Edge device, you must create and activate your Tape Gateway from the same AWS Region\. Attempting to create and activate the Tape Gateway in any AWS Region other than the one where the Snowball Edge device was ordered is not supported, and will not work\.
+ To back up your tape data using a Tape Gateway on a Snowball Edge device, connect the virtual tape devices on the gateway to a Windows or Linux client on your network, and then access them using your preferred backup software\. For more information about connecting the gateway to a client and testing it with your backup software, see [Using Your Tape Gateway](GettingStarted-create-tape-gateway.md)\.
+ When a virtual tape is in the **Available** state in the Storage Gateway console, it is ready to be mounted using your preferred backup software, and its full capacity is reserved in physical storage on the Snowball Edge device\. When you eject a virtual tape, its status changes from **Available** to **In transit to VTS**, and only the specific amount of data written to the tape remains reserved on the Snowball Edge device\. You don't need to eject virtual tapes from your backup software before shipping your Snowball Edge device back to AWS\. Any virtual tapes left in the **Available** state are automatically ejected during the data\-transfer process at the AWS facility\.
+ After you copy the data to your Snowball Edge device, you can schedule a pickup appointment to ship the device back to AWS\. The E Ink shipping label is automatically updated to ensure that the device is sent to the correct AWS facility\. For more information, see [Shipping an AWS Snowball Edge Device](https://docs.aws.amazon.com/snowball/latest/developer-guide/mailing-storage.html) in the *AWS Snowball Edge Developer Guide*\.

  You can track the device by using Amazon Simple Notification Service \(Amazon SNS\) generated text messages and emails\.
+ After your tape data is successfully transferred to the AWS Cloud and your Snowball Edge job is complete, you must use the Storage Gateway console to manually delete the associated Tape Gateway\.
+ In rare cases, data corruption or other technical difficulties might prevent AWS from transferring specific virtual tapes to the AWS Cloud after receiving your Snowball Edge device\. In such a case, you must use the Storage Gateway console to delete the virtual tapes that failed to transfer before you can re\-attempt the transfer on another Snowball Edge device\.
+ A Tape Gateway on a Snowball Edge device supports only importing virtual tape data to AWS, and cannot be used to access data that has already been imported\. To access your imported tape data, set up a standard Tape Gateway hosted on a virtual machine, hardware appliance, or Amazon EC2 instance, and then transfer the data from AWS over a network connection\.
+ A Snowball Edge device that is configured for Tape Gateway is not intended for use with other services or resources, such as Amazon S3, Network File System \(NFS\) file shares, AWS Lambda, or Amazon EC2\. To use those services or resources, you must create a new Snowball Edge job to order a separate, appropriately configured device\. For instructions, see [Creating an AWS Snowball Edge Job](https://docs.aws.amazon.com/snowball/latest/developer-guide/create-job-common.html) in the *AWS Snowball Edge Developer Guide*\.
+ To troubleshoot a Tape Gateway on a Snowball Edge device, or if directed to do so by AWS Support, you might need to connect to your gateway's local console\. The local console is a configuration interface that runs on the Snowball Edge device that's hosting your gateway\. You can use this local console to perform maintenance tasks specific to the gateway on that device\. For more information, see [Performing Maintenance Tasks on the Local Console](manage-on-premises.md)\.

  To access the local console for a Tape Gateway on a Snowball Edge device:

  1. From the command prompt on a computer connected to the same local network as your device, run the following command: 

     `ssh user_name@xxx.xxx.xxx.xxx`

     To run this command, replace *`user_name`* with your local console user name, and replace *`xxx.xxx.xxx.xxx`* with the Tape Gateway IP address that you obtained when you launched the Tape Gateway on your Snowball Edge device\. For instructions on how to obtain the Tape Gateway IP address, see [Deploying a Snowball Edge Device with a Tape Gateway](https://docs.aws.amazon.com/snowball/latest/developer-guide/using-snowball-tape-gateway.html#deploying-snowball-tape-gateway) in the *AWS Snowball Edge Developer Guide*\.

  1. Enter your password\.
**Note**  
If this is your first time logging in to the local console on this device, the default user name is *`admin`*, and the default password is *`password`*\. Change the default password immediately after you log in\. For more information, see [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.

## Using the Storage Gateway API with Tape Gateway on Snowball Edge<a name="using-sgw-api-tape-gateway-snowball"></a>

The Storage Gateway API works the same for a Tape Gateway on a Snowball Edge device as it does for a standard Tape Gateway, with the following exceptions:
+ The `GatewayType` parameter value for a Tape Gateway on a Snowball Edge device is `VTL_SNOW`\. You must specify this value for the `GatewayType` parameter when using the `ActivateGateway` API operation to activate a Tape Gateway on a Snowball Edge device\.
+ The `HostEnvironment` parameter value for a Tape Gateway on a Snowball Edge device is `SNOWBALL`\. You can activate a gateway that has the `VTL_SNOW` value for the `GatewayType` parameter only on a device that has the `SNOWBALL` value for the `HostEnvironment` parameter\.
+ The `HostEnvironmentId` parameter specifies the automatically generated `JobID` that's associated with the Snowball Edge device that's hosting the Tape Gateway\.
+ You can use the `DescribeGatewayInformation` and `ListGateways` API operations to return information about your Tape Gateway on a Snowball Edge device, including its `GatewayType`, `HostEnvironment`, and `HostEnvironmentId` parameter values\.

For more detailed information about the Storage Gateway API, see the [AWS Storage Gateway API Reference](https://docs.aws.amazon.com/storagegateway/latest/APIReference/Welcome.html)\.