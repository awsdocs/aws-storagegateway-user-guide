--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Managing Bandwidth for Your Tape or Volume Gateway<a name="MaintenanceUpdateBandwidth-common"></a>

You can limit \(or throttle\) the upload throughput from the gateway to AWS or the download throughput from AWS to your gateway\. Using bandwidth throttling helps you to control the amount of network bandwidth used by your gateway\. By default, an activated gateway has no rate limits on upload or download\.

You can specify the rate limit by using the AWS Management Console, or programmatically by using either the Storage Gateway API \(see [UpdateBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)\) or an AWS Software Development Kit \(SDK\)\. By throttling bandwidth programmatically, you can change limits automatically throughout the day—for example, by scheduling tasks to change the bandwidth\.

You can also define schedule\-based bandwidth throttling for your gateway\. You schedule bandwidth throttling by defining one or more bandwidth\-rate\-limit intervals\. For more information, see [Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console](#SchedulingBandwidthThrottling)\. 

Configuring a single setting for bandwidth throttling is the functional equivalent of defining a schedule with a single bandwidth\-rate\-limit interval set for **Everyday**, with a **Start time** of `00:00` and an **End time** of `23:59`\. 

**Note**  
The information in this section is specific to Tape and Volume gateways\. To manage bandwidth for an Amazon S3 File Gateway, see [Managing Bandwidth for Your Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/MaintenanceUpdateBandwidth-common.html)\. Bandwidth\-rate limits are currently not supported for Amazon FSx File Gateway\.

**Topics**
+ [Changing Bandwidth Throttling Using the Storage Gateway Console](#MaintenanceUpdateBandwidthConsole-common)
+ [Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console](#SchedulingBandwidthThrottling)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java](#MaintenanceUpdateBandwidthJava-common)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for \.NET](#MaintenanceUpdateBandwidthDotNet-common)
+ [Updating Gateway Bandwidth\-Rate Limits Using the AWS Tools for Windows PowerShell](#MaintenanceUpdateBandwidthPowerShell-common)

## Changing Bandwidth Throttling Using the Storage Gateway Console<a name="MaintenanceUpdateBandwidthConsole-common"></a>

The following procedure shows how to change a gateway's bandwidth throttling from the Storage Gateway console\.

**To change a gateway's bandwidth throttling using the console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the left navigation pane, choose **Gateways**, and then choose the gateway that you want to manage\.

1. For **Actions**, choose **Edit bandwidth limit**\.

1. In the **Edit rate limits** dialog box, enter new limit values, and then choose **Save**\. Your changes appear in the **Details** tab for your gateway\.

## Schedule\-Based Bandwidth Throttling Using the Storage Gateway Console<a name="SchedulingBandwidthThrottling"></a>

The following procedure shows how to schedule changes to a gateway's bandwidth throttling using the Storage Gateway console\.

**To add or modify a schedule for gateway bandwidth throttling**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the left navigation pane, choose **Gateways**, and then choose the gateway that you want to manage\.

1. For **Actions**, choose **Edit bandwidth rate limit schedule**\.

   The gateway's bandwidth\-rate\-limit schedule is displayed in the **Edit bandwidth rate limit schedule** dialog box\. By default, a new gateway bandwidth\-rate\-limit schedule is empty\. 

1. In the **Edit bandwidth rate limit schedule** dialog box, choose **Add new item** to add a new bandwidth\-rate\-limit interval\. Enter the following information for each bandwidth\-rate\-limit interval:
   + **Days of week** – You can create the bandwidth\-rate\-limit interval for weekdays \(Monday through Friday\), for weekends \(Saturday and Sunday\), for every day of the week, or for one or more specific days of the week\.
   + **Start time** – Enter the start time for the bandwidth interval in the gateway's local timezone, using the HH:MM format\.
**Note**  
Your bandwidth\-rate\-limit interval begins at the start of the minute that you specify here\.
   + **End time** – Enter the end time for the bandwidth\-rate\-limit interval in the gateway's local time zone, using the HH:MM format\. 
**Important**  
The bandwidth\-rate\-limit interval ends at the end of the minute specified here\. To schedule an interval that ends at the end of an hour, enter **59**\.  
To schedule consecutive continuous intervals, transitioning at the start of the hour, with no interruption between the intervals, enter **59** for the end minute of the first interval\. Enter **00** for the start minute of the succeeding interval\. 
   + **Download rate** – Enter the download rate limit, in kilobits per second \(Kbps\), or select **No limit** to disable bandwidth throttling for downloading\. The minimum value for the download rate is 100 Kbps\. 
   + **Upload rate** – Enter the upload rate limit, in Kbps, or select **No limit** to disable bandwidth throttling for uploading\. The minimum value for the upload rate is 50 Kbps\. 

   To modify your bandwidth\-rate\-limit intervals, you can enter revised values for the interval parameters\. 

   To remove your bandwidth\-rate\-limit intervals, you can choose **Remove** to the right of the interval to be deleted\.

   When your changes are complete, choose **Save**\.

1. Continue adding bandwidth\-rate\-limit intervals by choosing **Add new item** and entering the day, the start and end times, and the download and upload rate limits\. 
**Important**  
 Bandwidth\-rate\-limit intervals cannot overlap\. The start time of an interval must occur after the end time of a preceding interval, and before the start time of a following interval\. 

1. After entering all bandwidth\-rate\-limit intervals, choose **Save changes** to save your bandwidth\-rate\-limit schedule\.

When the bandwidth\-rate\-limit schedule is successfully updated, you can see the current download and upload rate limits in the **Details** panel for the gateway\.

## Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java<a name="MaintenanceUpdateBandwidthJava-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust your limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits using the AWS SDK for Java\. To use the example code, you should be familiar with running a Java console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/java-dg-setup.html) in the *AWS SDK for Java Developer Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for Java**  
The following Java code example updates a gateway's bandwidth\-rate limits\. To use this example code, you must provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints that you can use with Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.  

```
import java.io.IOException;

import com.amazonaws.AmazonClientException;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.storagegateway.AWSStorageGatewayClient;
import com.amazonaws.services.storagegateway.model.UpdateBandwidthRateLimitRequest;
import com.amazonaws.services.storagegateway.model.UpdateBandwidthRateLimitResult;


public class UpdateBandwidthExample {

    public static AWSStorageGatewayClient sgClient;
    
    // The gatewayARN
    public static String gatewayARN = "*** provide gateway ARN ***";

    // The endpoint
    static String serviceURL = "https://storagegateway.us-east-1.amazonaws.com";
    
    // Rates
    static long uploadRate = 51200;  // Bits per second, minimum 51200
    static long downloadRate = 102400;   // Bits per second, minimum 102400
    
    public static void main(String[] args) throws IOException {

        // Create a storage gateway client
        sgClient = new AWSStorageGatewayClient(new PropertiesCredentials(
                UpdateBandwidthExample.class.getResourceAsStream("AwsCredentials.properties")));    
        sgClient.setEndpoint(serviceURL);
        
        UpdateBandwidth(gatewayARN, uploadRate, downloadRate);
        
    }

    private static void UpdateBandwidth(String gatewayARN2, long uploadRate2,
            long downloadRate2) {
        try
        {
            UpdateBandwidthRateLimitRequest updateBandwidthRateLimitRequest =
                new UpdateBandwidthRateLimitRequest()
                .withGatewayARN(gatewayARN)
                .withAverageDownloadRateLimitInBitsPerSec(downloadRate)
                .withAverageUploadRateLimitInBitsPerSec(uploadRate);

            UpdateBandwidthRateLimitResult updateBandwidthRateLimitResult = sgClient.updateBandwidthRateLimit(updateBandwidthRateLimitRequest);
            String returnGatewayARN = updateBandwidthRateLimitResult.getGatewayARN();
            System.out.println("Updated the bandwidth rate limits of " + returnGatewayARN);
            System.out.println("Upload bandwidth limit = " + uploadRate + " bits per second");
            System.out.println("Download bandwidth limit = " + downloadRate + " bits per second");
        }
        catch (AmazonClientException ex)
        {
            System.err.println("Error updating gateway bandwith.\n" + ex.toString());
        }
    }        
}
```

## Updating Gateway Bandwidth\-Rate Limits Using the AWS SDK for \.NET<a name="MaintenanceUpdateBandwidthDotNet-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust your limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits by using the AWS SDK for \.NET\. To use the example code, you should be familiar with running a \.NET console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits by Using the AWS SDK for \.NET**  
The following C\# code example updates a gateway's bandwidth\-rate limits\. To use this example code, you must provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints that you can use with Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.  

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Amazon.StorageGateway;
using Amazon.StorageGateway.Model;

namespace AWSStorageGateway
{
    class UpdateBandwidthExample
    {
        static AmazonStorageGatewayClient sgClient;
        static AmazonStorageGatewayConfig sgConfig;

        // The gatewayARN
        public static String gatewayARN = "*** provide gateway ARN ***";

        // The endpoint
        static String serviceURL = "https://storagegateway.us-east-1.amazonaws.com";

        // Rates
        static long uploadRate = 51200;  // Bits per second, minimum 51200
        static long downloadRate = 102400;   // Bits per second, minimum 102400

        public static void Main(string[] args)
        {
            // Create a storage gateway client
            sgConfig = new AmazonStorageGatewayConfig();
            sgConfig.ServiceURL = serviceURL;
            sgClient = new AmazonStorageGatewayClient(sgConfig);

            UpdateBandwidth(gatewayARN, uploadRate, downloadRate);

            Console.WriteLine("\nTo continue, press Enter.");
            Console.Read();
        }

        public static void UpdateBandwidth(string gatewayARN, long uploadRate, long downloadRate)
        {
            try
            {
                UpdateBandwidthRateLimitRequest updateBandwidthRateLimitRequest =
                    new UpdateBandwidthRateLimitRequest()
                    .WithGatewayARN(gatewayARN)
                    .WithAverageDownloadRateLimitInBitsPerSec(downloadRate)
                    .WithAverageUploadRateLimitInBitsPerSec(uploadRate);

                UpdateBandwidthRateLimitResponse updateBandwidthRateLimitResponse = sgClient.UpdateBandwidthRateLimit(updateBandwidthRateLimitRequest);
                String returnGatewayARN = updateBandwidthRateLimitResponse.UpdateBandwidthRateLimitResult.GatewayARN;
                Console.WriteLine("Updated the bandwidth rate limits of " + returnGatewayARN);
                Console.WriteLine("Upload bandwidth limit = " + uploadRate + " bits per second");
                Console.WriteLine("Download bandwidth limit = " + downloadRate + " bits per second");
            }
            catch (AmazonStorageGatewayException ex)
            {
                Console.WriteLine("Error updating gateway bandwith.\n" + ex.ToString());
            }
        }
    }
}
```

## Updating Gateway Bandwidth\-Rate Limits Using the AWS Tools for Windows PowerShell<a name="MaintenanceUpdateBandwidthPowerShell-common"></a>

By updating bandwidth\-rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth\-rate limits using the AWS Tools for Windows PowerShell\. To use the example code, you should be familiar with running a PowerShell script\. For more information, see [Getting Started](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html) in the *AWS Tools for Windows PowerShell User Guide*\. 

**Example : Updating Gateway Bandwidth\-Rate Limits by Using the AWS Tools for Windows PowerShell**  
The following PowerShell script example updates a gateway's bandwidth\-rate limits\. To use this example script, you must provide your gateway Amazon Resource Name \(ARN\), and the upload and download limits\.   

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