# Creating Gateway in a Virtual Private Cloud<a name="create-gateway-vpc"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a connection between your virtual private cloud \(VPC\) and file gateway\. You can then use this gateway to establish a connection between your IT environment and the AWS storage infrastructure without going over the public internet\.

Using Amazon VPC, you can launch AWS resources in a custom virtual network\. You can use a VPC to control your network settings, such as the IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide\.*

In the next section, you can find instructions on how to connect your VPC to a file gateway\. First, you define an interface VPC endpoint, which enables you to connect your VPC to other AWS services\. The endpoint provides reliable, scalable connectivity to services without requiring an internet gateway, network address translation \(NAT\) instance, or virtual private network \(VPN\) connection\. For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide\.*

In the next section, you can also see how to create a file gateway using a VPC endpoint\. Doing this enables file gateway to store and retrieve objects in Amazon S3, even though the network is disconnected from the public internet\. 

**Topics**
+ [Create a VPC Endpoint](#create-vpc-endpointx)

## Create a VPC Endpoint<a name="create-vpc-endpointx"></a>

In the following walkthrough, you create a gateway that is in a VPC and not accessible over the public internet\. To do this, you take the following steps:
+ Create a VPC endpoint\.
+ Create and configure your gateway to use the VPC endpoint\.<a name="create-vpc-steps"></a>

**To create a VPC endpoint for a gateway**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, choose **AWS Services** for **Service category**\.

1. For **Service Name**, choose `com.amazonaws.region.storagegateway` and then choose **Create endpoint**\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is selected\.

1. For **Security group**, choose the security group that you want to use for your VPC\. You can accept the default security group\.

1. Choose **Create endpoint**\. The initial state of the endpoint is **pending**\. When the endpoint is created, take note of the ID of the VPC endpoint that you just created\. 

1. In the navigation pane, choose **Endpoints** and copy your endpoint\.

Now that you have a VPC endpoint, you can create your gateway\. The following instructions show you how to create a gateway using a VPC endpoint\.

**To create a gateway and configure it to use a VPC endpoint**

1. Open the AWS Management Console at [http://console\.aws\.amazon\.com/storagegateway/home](http://console.aws.amazon.com/storagegateway/home), and choose the AWS Region that you want to create your gateway in\.

   If you have previously created a gateway in this AWS Region, the console shows your gateway\. Otherwise, the service homepage appears\.

1. Choose a gateway type\.

1. Choose a host platform\.

1. Choose a service endpoint\.
**Note**  
You can associate a VPC endpoint with one gateway at a time\.

1. Use your gateway's IP address to connect to gateway

1. Activate your gateway\.

1. Configure local disks\.

For step\-by\-step instructions on how to create a gateway, see the following:
+ File gateway—[Creating a Gateway](create-gateway-file.md)
+ Volume gateway—[Creating a Gateway](create-volume-gateway.md)
+ Tape gateway—[Creating a Gateway](create-gateway-vtl.md)