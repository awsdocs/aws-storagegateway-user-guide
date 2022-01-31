--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Using AWS Direct Connect with Storage Gateway<a name="using-dx"></a>

AWS Direct Connect links your internal network to the Amazon Web Services Cloud\. By using AWS Direct Connect with Storage Gateway, you can create a connection for high\-throughput workload needs, providing a dedicated network connection between your on\-premises gateway and AWS\. 

Storage Gateway uses public endpoints\. With an AWS Direct Connect connection in place, you can create a public virtual interface to allow traffic to be routed to the Storage Gateway endpoints\. The public virtual interface bypasses internet service providers in your network path\. The Storage Gateway service public endpoint can be in the same AWS Region as the AWS Direct Connect location, or it can be in a different AWS Region\. 

The following illustration shows an example of how AWS Direct Connect works with Storage Gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/DirectConnect3.png)

The following procedure assumes that you have created a functioning gateway\.

**To use AWS Direct Connect with Storage Gateway**

1. Create and establish an AWS Direct Connect connection between your on\-premises data center and your Storage Gateway endpoint\. For more information about how to create a connection, see [Getting Started with ](https://docs.aws.amazon.com/directconnect/latest/UserGuide/getting_started.html) in the *AWS Direct Connect User Guide\.*

1. Connect your on\-premises Storage Gateway appliance to the AWS Direct Connect router\. 

1. Create a public virtual interface, and configure your on\-premises router accordingly\. For more information, see [Creating a Virtual Interface](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-vif.html) in the *AWS Direct Connect User Guide\.*

For details about AWS Direct Connect, see [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) in the *AWS Direct Connect User Guide*\.