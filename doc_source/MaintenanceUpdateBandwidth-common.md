# Managing Bandwidth for Your Gateway<a name="MaintenanceUpdateBandwidth-common"></a>

You can limit \(or throttle\) the upload throughput from the gateway to AWS or the download throughput from your AWS to your gateway\. Using bandwidth throttling helps you to control the amount of network bandwidth used by your gateway\. By default, an activated gateway has no rate limits on upload or download\.

You can specify the rate limit by using the AWS Management Console, or programmatically by using either the AWS Storage Gateway API \(see [UpdateBandwidthRateLimit](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)\) or an AWS Software Development Kit \(SDK\)\. By throttling bandwidth programmatically, you can change limits automatically throughout the day**—**for example, by scheduling tasks to change the bandwidth\. As described directly following, you can change these limits by using the AWS Storage Gateway console\. Or, for information about changing bandwidth rate limits programmatically, see the following topics\.


+ [Updating Gateway Bandwidth Rate Limits Using the AWS SDK for Java](#MaintenanceUpdateBandwidthJava-common)
+ [Updating Gateway Bandwidth Rate Limits Using the AWS SDK for \.NET](#MaintenanceUpdateBandwidthDotNet-common)
+ [Updating Gateway Bandwidth Rate Limits Using the AWS Tools for Windows PowerShell](#MaintenanceUpdateBandwidthPowerShell-common)

**Note**  
Configuring bandwidth rate limit is currently not supported in the file gateway type\.

**To change a gateway's bandwidth throttling using the console**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway you want to manage\.

1. On the **Actions** menu, choose **Edit Bandwidth Rate Limit**\.

1. In the **Edit Rate Limits** dialog box, type new limit values, and then choose **Save**\. Your changes appear in the **Details** tab for your gateway\.

## Updating Gateway Bandwidth Rate Limits Using the AWS SDK for Java<a name="MaintenanceUpdateBandwidthJava-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits using the AWS SDK for Java\. To use the example code, you should be familiar with running a Java console application\. For more information, see [Getting Started](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-setup.html) in the *AWS SDK for Java Developer Guide*\. 

**Example : Updating Gateway Bandwidth Limits Using the AWS SDK for Java**  
The following Java code example updates a gateway's bandwidth rate limits\. You need to update the code and provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints you can use with AWS Storage Gateway, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#sg_region) in the *AWS General Reference*\.  

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
                ListDeleteVolumeSnapshotsExample.class.getResourceAsStream("AwsCredentials.properties")));    
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

## Updating Gateway Bandwidth Rate Limits Using the AWS SDK for \.NET<a name="MaintenanceUpdateBandwidthDotNet-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits by using the AWS Software Development Kit \(SDK\) for \.NET\. To use the example code, you should be familiar with running a \.NET console application\. For more information, see [Getting Started](http://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\. 

**Example : Updating Gateway Bandwidth Limits by Using the AWS SDK for \.NET**  
The following C\# code example updates a gateway's bandwidth rate limits\. You need to update the code and provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the upload and download limits\. For a list of AWS service endpoints you can use with AWS Storage Gateway, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#sg_region) in the *AWS General Reference*\.  

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

## Updating Gateway Bandwidth Rate Limits Using the AWS Tools for Windows PowerShell<a name="MaintenanceUpdateBandwidthPowerShell-common"></a>

By updating bandwidth rate limits programmatically, you can adjust limits automatically over a period of time—for example, by using scheduled tasks\. The following example demonstrates how to update a gateway's bandwidth rate limits using the AWS Tools for Windows PowerShell\. To use the example code, you should be familiar with running a PowerShell script\. For more information, see [Getting Started](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html) in the *AWS Tools for Windows PowerShell User Guide*\. 

**Example : Updating Gateway Bandwidth Limits by Using the AWS Tools for Windows PowerShell**  
The following PowerShell script example updates a gateway's bandwidth rate limits\. You need to update the script and provide your gateway Amazon Resource Name \(ARN\), and the upload and download limits\.   

```
<#
.DESCRIPTION
    Update Gateway bandwidth limits.
    
.NOTES
    PREREQUISITES:
    1) AWS Tools for PowerShell from http://aws.amazon.com/powershell/
    2) Credentials and region stored in session using Initialize-AWSDefault.
    For more info see, http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html

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