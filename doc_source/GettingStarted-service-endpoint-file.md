# Choose a service endpoint<a name="GettingStarted-service-endpoint-file"></a>

In this step, you choose a service endpoint for your gateway in Storage Gateway\.

You can activate your gateway using:
+ A public service endpoint and have your gateway communicate with AWS storage services over the public internet\.
+ A Federal Information Processing Standards \(FIPS\) compliant public service endpoint and have your gateway communicate with AWS storage services over the public internet\.
+ A public service endpoint and have your gateway communicate with AWS storage services using a virtual private cloud \(VPC\) endpoint, which is private\.

**Note**  
If you use a VPC endpoint, all VPC endpoint communication from your gateway to AWS services occurs through the public service endpoint using your VPC in AWS\.

------
#### [ New console ]

**To choose a service endpoint**

1. For **Select service endpoint**, choose one of the following:
   + To have your gateway access AWS services over the public internet using a public service endpoint, choose **Public**\.
   + To have your gateway access AWS services over the public internet using a public service endpoint that complies with FIPS, choose **FIPS**\.

     If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\. 
   + To have your gateway access AWS services over a private VPC endpoint connection using a public service endpoint, choose **VPC**\.
**Note**  
The FIPS service endpoint is only available in some AWS Regions\. For more information, see [Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

   This procedure assumes that you are activating your gateway with a public endpoint\. For information about how to activate a gateway using a VPC endpoint, see [Activating a gateway in a virtual private cloud](gateway-private-link.md)\.

1. Choose **Next** to connect and activate your gateway\.

------
#### [ Original console ]

**To choose a service endpoint**

1. For **Endpoint type**, choose one of the following:
   + To have your gateway access AWS services over the public internet using a public service endpoint, choose **Public**\.
   + To have your gateway access AWS services over the public internet using a public service endpoint that complies with FIPS, choose **FIPS**\.

     If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\. 
   + To have your gateway access AWS services over a private VPC endpoint connection using a public service endpoint, choose **VPC**\.
**Note**  
The FIPS service endpoint is only available in some AWS Regions\. For more information, see [Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

   This procedure assumes that you are activating your gateway with a public endpoint\. For information about how to activate a gateway using a VPC endpoint, see [Activating a gateway in a virtual private cloud](gateway-private-link.md)\.

1. Choose **Next** to connect and activate your gateway\.

------