# Performing Tasks on the Amazon EC2 Local Console \(Volume and Tape Gateways\)<a name="ec2-local-console-common"></a>

Some maintenance tasks require that you log in to the local console when running a gateway deployed on an Amazon EC2 instance\. In this section, you can find information about how to log in to the local console and perform maintenance tasks\.

**Topics**
+ [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)
+ [Routing Your Gateway Deployed on Amazon EC2 Through a Proxy](#EC2_MaintenanceRoutingProxy-common)
+ [Testing Your Gateway Connectivity to the Internet](#EC2_MaintenanceTestGatewayConnectivity-common)
+ [Running Storage Gateway Commands on the Local Console](#EC2_MaintenanceGatewayConsole-common)
+ [Viewing Your Gateway System Resource Status](#EC2_system-resource-check-common)

## Logging In to Your Amazon EC2 Gateway Local Console<a name="EC2_MaintenanceConsoleWindow-common"></a>

You can connect to your Amazon EC2 instance by using a Secure Shell \(SSH\) client\. For detailed information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide//AccessingInstances.html) in the *Amazon EC2 User Guide*\. To connect this way, you will need the SSH key pair you specified when you launched the instance\. For information about Amazon EC2 key pairs, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide//ec2-key-pairs.html) in the *Amazon EC2 User Guide\.*<a name="EC2_MaintenanceConsoleWindowMenu-common"></a>

**To log in to the gateway local console**

1. Log in to your local console\. If you are connecting to your EC2 instance from a Windows computer, log in as *admin*\.

1. After you log in, you see the **AWS Storage Gateway Configuration** main menu, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-local-console-common.html)

To shut down the gateway, type **0**\. 

To exit the configuration session, type **x** to exit the menu\. 

## Routing Your Gateway Deployed on Amazon EC2 Through a Proxy<a name="EC2_MaintenanceRoutingProxy-common"></a>

AWS Storage Gateway supports the configuration of a Socket Secure version 5 \(SOCKS5\) proxy between your gateway deployed on Amazon EC2 and AWS\.

**Note**  
The only proxy configuration AWS Storage Gateway supports is SOCKS5\.

If your gateway must use a proxy server to communicate to the Internet, then you need to configure the SOCKS proxy settings for your gateway\. You do this by specifying an IP address and port number for the host running your proxy\. After you do so, AWS Storage Gateway will route all HyperText Transfer Protocol Secure \(HTTPS\) traffic through your proxy server\. 

**To route your gateway Internet traffic through a local proxy server**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. On the **AWS Storage Gateway Configuration** main menu, type **1** to begin configuring the SOCKS proxy\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

1. Choose one of the following options in the **AWS Storage Gateway SOCKS Proxy Configuration** menu:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_77.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-local-console-common.html)

## Testing Your Gateway Connectivity to the Internet<a name="EC2_MaintenanceTestGatewayConnectivity-common"></a>

You can use your gateway's local console to test your Internet connection\. This test can be useful when you are troubleshooting network issues with your gateway\.

**To test your gateway's connection to the Internet**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. In the **AWS Storage Gateway Configuration** main menu, type **2** to begin testing network connectivity\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

   The console displays the available regions\. 

1. Select the region you want to test\. Following are the available regions for gateways deployed an EC2 instance\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-local-console-common.html)

   Each endpoint in the region you select displays either a **\[PASSED\]** or **\[FAILED\]** message, as shown following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-local-console-common.html)

## Running Storage Gateway Commands on the Local Console<a name="EC2_MaintenanceGatewayConsole-common"></a>

The AWS Storage Gateway console helps provide a secure environment for configuring and diagnosing issues with your gateway\. Using the console commands, you can perform maintenance tasks such as saving routing tables or connecting to AWS Support\. 

**To run a configuration or diagnostic command**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. In the **AWS Storage Gateway Configuration** main menu, type **3** for **Gateway Console**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

1. In the Storage Gateway console, type **h**, and then press the **Return** key\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGLocalConsole.png)

   The console displays the **Available Commands** menu with the available commands\. After the menu, a **Gateway Console** prompt appears, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_AvailableLocalConsoleCommands.png)

1. To learn about a command, type **man** \+ ***command name*** at the **Gateway Console** prompt\.

## Viewing Your Gateway System Resource Status<a name="EC2_system-resource-check-common"></a>

When your gateway starts, it checks its virtual CPU cores, root volume size, and RAM and determines whether these system resources are sufficient for your gateway to function properly\. You can view the results of this check on the gateway's local console\.

**To view the status of a system resource check**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. In the **AWS Storage Gateway Configuration** main menu, type **4** to view the results of a system resource check\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_LocalConsole-StartPage.png)

   The console displays an **\[OK**\], **\[WARNING\]**, or **\[FAIL\]** message for each resource as described in the table following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/ec2-local-console-common.html)

   The console also displays the number of errors and warnings next to the resource check menu option\.

   The following screenshot shows a **\[FAIL\]** message and the accompanying error message\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/EC2_new-gateway-console-error.png)