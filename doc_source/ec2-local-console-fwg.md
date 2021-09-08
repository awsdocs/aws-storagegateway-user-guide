# Performing tasks on the Amazon EC2 local console \(file gateway\)<a name="ec2-local-console-fwg"></a>

Some maintenance tasks require that you log in to the local console when running a gateway deployed on an Amazon EC2 instance\. In this section, you can find information about how to log in to the local console and perform maintenance tasks\.

**Topics**
+ [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)
+ [Routing your gateway deployed on EC2 through an HTTP proxy](#EC2_MaintenanceRoutingProxy-fgw)
+ [Configuring your gateway network settings](#EC2-MaintenanceConfiguringStaticIP-fgw)
+ [Testing your gateway connectivity to the internet](#EC2_MaintenanceTestGatewayConnectivity-fgw)
+ [Viewing your gateway system resource status](#EC2_system-resource-check-fgw)
+ [Running Storage Gateway commands on the local console](#EC2_MaintenanceGatewayConsole-fgw)

## Logging in to your Amazon EC2 gateway local console<a name="EC2_MaintenanceConsoleWindow-fgw"></a>

You can connect to your Amazon EC2 instance by using a Secure Shell \(SSH\) client\. For detailed information, see [Connect to your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide*\. To connect this way, you need the SSH key pair that you specified when you launched your instance\. For information about Amazon EC2 key pairs, see [Amazon EC2 key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide\.*<a name="EC2_MaintenanceConsoleWindowMenu-fgw"></a>

**To log in to the gateway local console**

1. Log in to your local console\. If you are connecting to your EC2 instance from a Windows computer, log in as *admin*\.

1. After you log in, you see the **AWS Appliance Activation \- Configuration** main menu, as shown in the following screenshot\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-local-console-fwg.html)

To shut down the gateway, enter **0**\. 

To exit the configuration session, enter **x** to exit the menu\. 

## Routing your gateway deployed on EC2 through an HTTP proxy<a name="EC2_MaintenanceRoutingProxy-fgw"></a>

Storage Gateway supports the configuration of a Socket Secure version 5 \(SOCKS5\) proxy between your gateway deployed on Amazon EC2 and AWS\.

If your gateway must use a proxy server to communicate to the internet, then you need to configure the HTTP proxy settings for your gateway\. You do this by specifying an IP address and port number for the host running your proxy\. After you do so, Storage Gateway routes all AWS endpoint traffic through your proxy server\. Communications between the gateway and endpoints is encrypted, even when using the HTTP proxy\.

**To route your gateway internet traffic through a local proxy server**

1. Log in to your gateway's local console\. For instructions, see [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)\.

1. On the **AWS Appliance Activation \- Configuration** main menu, enter **1** to begin configuring the HTTP proxy\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)

1. Choose one of the following options in the ****AWS Appliance Activation \- Configuration** HTTP Proxy Configuration** menu\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-1.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-local-console-fwg.html)

## Configuring your gateway network settings<a name="EC2-MaintenanceConfiguringStaticIP-fgw"></a>

You can view and configure your Domain Name Server \(DNS\) settings through the local console\.

**To configure your gateway to use static IP addresses**

1. Log in to your gateway's local console\. For instructions, see [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)\.

1. On the **AWS Appliance Activation \- Configuration** main menu, enter **2** to begin configuring your DNS server\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)

1. On the ** Network Configuration** menu, choose one of the following options\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-2.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-local-console-fwg.html)

## Testing your gateway connectivity to the internet<a name="EC2_MaintenanceTestGatewayConnectivity-fgw"></a>

You can use your gateway's local console to test your internet connection\. This test can be useful when you are troubleshooting network issues with your gateway\.

**To test your gateway's connection to the internet**

1. Log in to your gateway's local console\. For instructions, see [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)\.

1. In the **Storage Gateway Configuration** main menu, enter **3** to begin testing network connectivity\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)

   The console displays the available AWS Regions\. 

1. Choose option **1** for Storage Gateway\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-3.png)

   The console displays the available AWS Regions for Storage Gateway\.

1. Choose the AWS Region that you want to test\. For example, us\-east\-2\. For supported AWS Regions and a list of AWS service endpoints you can use with Storage Gateway, see [Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

   Each endpoint in the AWS Region that you choose displays either a **\[PASSED\]** or **\[FAILED\]** message, as shown following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-local-console-fwg.html)

## Viewing your gateway system resource status<a name="EC2_system-resource-check-fgw"></a>

When your gateway starts, it checks its virtual CPU cores, root volume size, and RAM\. It then determines whether these system resources are sufficient for your gateway to function properly\. You can view the results of this check on the gateway's local console\.

**To view the status of a system resource check**

1. Log in to your gateway's local console\. For instructions, see [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)\.

1. In the **Storage Gateway Configuration** main menu, enter **4** to view the results of a system resource check\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)

   The console displays an **\[OK**\], **\[WARNING\]**, or **\[FAIL\]** message for each resource as described in the table following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/ec2-local-console-fwg.html)

   The console also displays the number of errors and warnings next to the resource check menu option\.

## Running Storage Gateway commands on the local console<a name="EC2_MaintenanceGatewayConsole-fgw"></a>

The Storage Gateway console helps provide a secure environment for configuring and diagnosing issues with your gateway\. Using the console commands, you can perform maintenance tasks such as saving routing tables or connecting to Amazon Web Services Support\. 

**To run a configuration or diagnostic command**

1. Log in to your gateway's local console\. For instructions, see [Logging in to your Amazon EC2 gateway local console](#EC2_MaintenanceConsoleWindow-fgw)\.

1. In the **AWS Appliance Activation Configuration** main menu, enter **5** for **Gateway Console**\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-0.png)

1. In the command prompt, enter **h**, and then press the **Return** key\.

   The console displays the **AVAILABLE COMMANDS** menu with the available commands\. After the menu, a gateway console prompt appears, as shown in the following screenshot\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/local-console-file-ec2-5.png)

1. At the command prompt, enter the command that you want to use and follow the instructions\.

To learn about a command, enter the command name at the command prompt\.