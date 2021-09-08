# Managing Bandwidth for Your Gateway<a name="MaintenanceUpdateBandwidth-common"></a>

You can limit \(or throttle\) the upload throughput from the gateway to AWS or the download throughput from your AWS to your gateway\. Using bandwidth throttling helps you to control the amount of network bandwidth used by your gateway\. By default, an activated gateway has no rate limits on upload or download\.

You can specify the rate limit by using the AWS Management Console, or programmatically by using either the Storage Gateway API \(see [UpdateBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)\) or an AWS Software Development Kit \(SDK\)\. By throttling bandwidth programmatically, you can change limits automatically throughout the day**—**for example, by scheduling tasks to change the bandwidth\.

You can also define schedule\-based bandwidth throttling for your gateway\. You schedule bandwidth throttling by defining one or more bandwidth rate limit intervals\. For more information, see [Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console](#SchedulingBandwidthThrottling)\. 

The schedule\-based bandwidth throttling function is a superset of the changing bandwidth throttling function\. Configuring a single setting for gateway bandwidth throttling is the functional equivalent of defining a schedule with a single bandwidth rate interval set for **Everyday**, with a **Start time** of `00:00` and an **End time** of `23:59`\. 

**Note**  
Configuring bandwidth rate limit is currently not supported in the file gateway type\.

**Topics**
+ [Changing Bandwidth Throttling Using the Storage Gateway Console](#MaintenanceUpdateBandwidthConsole-common)
+ [Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console](#SchedulingBandwidthThrottling)
+ [Updating Gateway Bandwidth Rate Limits Using the AWS SDK for Java](#MaintenanceUpdateBandwidthJava-common)
+ [Updating Gateway Bandwidth Rate Limits Using the AWS SDK for \.NET](#MaintenanceUpdateBandwidthDotNet-common)
+ [Updating Gateway Bandwidth Rate Limits Using the AWS Tools for Windows PowerShell](#MaintenanceUpdateBandwidthPowerShell-common)

## Changing Bandwidth Throttling Using the Storage Gateway Console<a name="MaintenanceUpdateBandwidthConsole-common"></a>

The following procedure shows you how to change a gateway's bandwidth throttling from the Storage Gateway console\.

**To change a gateway's bandwidth throttling using the console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway that you want to manage\.

1. For **Actions**, choose **Edit Bandwidth Limit**\.

1. In the **Edit Rate Limits** dialog box, enter new limit values, and then choose **Save**\. Your changes appear in the **Details** tab for your gateway\.

## Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console<a name="SchedulingBandwidthThrottling"></a>

The following procedure shows you how to schedule changes to a gateway's bandwidth throttling using the Storage Gateway console\.

**To add or modify a schedule for gateway bandwidth throttling**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway that you want to manage\.

1. For **Actions**, choose **Edit bandwidth rate limit schedule**\.

   The gateway's bandwidth rate limit schedule is displayed in the **Edit bandwidth rate limit schedule** dialog box\. By default, a new gateway bandwidth rate limit schedule is empty\. 

1. In the **Edit bandwidth rate limit schedule** dialog box, choose **Add new entry** to add a new bandwidth rate limit interval\. Enter the following information for each bandwidth rate limit interval:
   + **Days of week** – You can create the bandwidth rate limit interval for weekdays \(Monday through Friday\), for weekends \(Saturday and Sunday\), for every day of the week, or for one or more specific days of the week\.
   + **Start time** – Enter the start time for the bandwidth interval, using the HH:MM format and the timezone offset from GMT for your gateway\. 
**Note**  
Your bandwidth rate limit interval begins at the start of the minute that you specify here\.
   + **End time** – Enter the end time for the bandwidth interval, using the HH:MM format and the timezone offset from GMT for your gateway\. 
**Important**  
The bandwidth rate limit interval ends at the end of the minute specified here\. To schedule an interval that ends at the end of an hour, enter **59**\.  
 To schedule consecutive continuous intervals, transitioning at the start of the hour, with no interruption between the intervals, enter **59** for the end minute of the first interval\. Enter **00** for the start minute of the succeeding interval\. 
   + **Download rate** – Enter the download rate limit, in kilobits/second, or select **No limit** to disable bandwidth throttling for downloading\. The minimum value for download rate is 100 kilobits/second\. 
   + **Upload rate** – Enter the upload rate limit, in kilobits/second, or select **No limit** to disable bandwidth throttling for uploading\. The minimum value for upload rate is 50 kilobits/second\. 
   + To modify bandwidth rate limit intervals, you can enter revised values for the interval parameters\. 

     To remove bandwidth rate limit intervals, you can choose the **cancel** icon \("**x**"\) to the right of the interval to be deleted\. 

     When changes are complete, choose **Save**\.

1. Continue adding bandwidth rate limit intervals by choosing **Add new entry** and entering the day, start and end times, and download and upload rate limits\. 
**Important**  
 Bandwidth rate limit intervals cannot overlap\. The start time of an interval must occur after the end time of a preceding interval, and before the start time of a following interval\. 

1. After entering all bandwidth rate limiting intervals, choose **Save** to save your bandwidth rate limit schedule\.

When the bandwidth rate limit schedule is successfully updated, you can see the current download and upload rate limits in the **Details** panel for the gateway\.

## Updating Gateway Bandwidth Rate Limits Using the AWS SDK for Java<a name="MaintenanceUpdateBandwidthJava-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits using the AWS SDK for Java\. To use the example code, you should be familiar with running a Java console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/java-dg-setup.html) in the *AWS SDK for Java Developer Guide*\. 

**Example : Updating Gateway Bandwidth Limits Using the AWS SDK for Java**  
The following Java code example updates a gateway's bandwidth rate limits\. You need to update the code and provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints you can use with Storage Gateway, see [Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

## Updating Gateway Bandwidth Rate Limits Using the AWS SDK for \.NET<a name="MaintenanceUpdateBandwidthDotNet-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits by using the AWS Software Development Kit \(SDK\) for \.NET\. To use the example code, you should be familiar with running a \.NET console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\. 

**Example : Updating Gateway Bandwidth Limits by Using the AWS SDK for \.NET**  
The following C\# code example updates a gateway's bandwidth rate limits\. You need to update the code and provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints you can use with Storage Gateway, see [Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

## Updating Gateway Bandwidth Rate Limits Using the AWS Tools for Windows PowerShell<a name="MaintenanceUpdateBandwidthPowerShell-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits using the AWS Tools for Windows PowerShell\. To use the example code, you should be familiar with running a PowerShell script\. For more information, see [Getting Started](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html) in the *AWS Tools for Windows PowerShell User Guide*\. 

**Example : Updating Gateway Bandwidth Limits by Using the AWS Tools for Windows PowerShell**  
The following PowerShell script example updates a gateway's bandwidth rate limits\. You need to update the script and provide your gateway Amazon Resource Name \(ARN\), and the upload and download limits\.   

```
<#
.DESCRIPTION
    Update Gateway bandwidth limits.
    
.NOTES
    PREREQUISITES:
    1) AWS Tools for PowerShell from https://aws.amazon.com/powershell/
    2) Credentials and region stored in session using Initialize-AWSDefault.
    For more info, see https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html
    

.EXAMPLE
    powershell.exe .\SG_UpdateBandwidth.ps1 
#>

$UploadBandwidthRate = 51200
$DownloadBandwidthRate = 102400
$gatewayARN = "*** provide gateway ARN ***"

#Update Bandwidth Rate Limits
Update-SGBandwidthRateLimit -GatewayARN $gatewayARN `
                            -AverageUploadRateLimitInBitsPerSec $UploadBandwidthRate `
                            -AverageDownloadRateLimitInBitsPerSec $DownloadBandwidthRate

$limits =  Get-SGBandwidthRateLimit -GatewayARN $gatewayARN

Write-Output("`nGateway: " + $gatewayARN);
Write-Output("`nNew Upload Rate: " + $limits.AverageUploadRateLimitInBitsPerSec)
Write-Output("`nNew Download Rate: " + $limits.AverageDownloadRateLimitInBitsPerSec)
```