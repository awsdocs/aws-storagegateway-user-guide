--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Launching a gateway<a name="appliance-launch-gateway"></a>

You can launch any of the three storage gateways on the appliance—file gateway, volume gateway \(cached\), or tape gateway\.

**To launch a gateway on your hardware appliance**

1. Sign in to the AWS Management Console and open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Hardware**\.

1. For **Actions**, choose **Launch Gateway**\.

1. For **Gateway Type**, choose the type of gateway you want to create\. 
**Note**  
Storage Gateway offers the following gateway types:  
[FSx File Gateway](https://docs.aws.amazon.com/filegateway/latest/filefsxw/what-is-file-fsxw.html)
[S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/what-is-file-s3.html)
[Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html#tape-gateway)
[Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html#volume-gateway)

1. For **Gateway name**, enter a name for your gateway\. Names can be 255 characters long and can't include a slash character\.

1. Choose **Launch gateway**\.

The Storage Gateway software for your chosen gateway type installs on the appliance\. It can take up to 5–10 minutes for a gateway to show up as **online** in the console\.

To assign a static IP address to your installed gateway, you next configure the gateway's network interfaces so your applications can use it\.

**Next step**

[Configuring an IP address for the gateway](appliance-configure-ip.md)