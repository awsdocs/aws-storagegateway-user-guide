--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Port Requirements<a name="Resource_Ports"></a>

Storage Gateway requires the following ports for its operation\. Some ports are common to and required by all gateway types\. Other ports are required by specific gateway types\. In this section, you can find an illustration and a list of the required ports for tape and volume gateways\.

**Volume Gateways and Tape Gateways**

The following illustration shows all of the ports you need to open for volume and tape gateway operation\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGWNetworkPorts16-volume-tape2.png)

The following ports are common to and required by all gateway types\.


|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  Storage Gateway VM  |  AWS  |  Transmission Control Protocol \(TCP\)  |  443 \(HTTPS\)  |  For communication from an Storage Gateway outbound VM to an AWS service endpoint\. For information about service endpoints, see [Allowing AWS Storage Gateway access through firewalls and routers](Requirements.md#allow-firewall-gateway-access)\.  | 
|  Your web browser  |  Storage Gateway VM  |  TCP  |  80 \(HTTP\)  |  By local systems to obtain the Storage Gateway activation key\. Port 80 is used only during activation of a Storage Gateway appliance\.  A Storage Gateway VM doesn't require port 80 to be publicly accessible\. The required level of access to port 80 depends on your network configuration\. If you activate your gateway from the Storage Gateway Management Console, the host from which you connect to the console must have access to your gateway’s port 80\.  | 
|  Storage Gateway VM  |  Domain Name Service \(DNS\) server  |  User Datagram Protocol \(UDP\)/UDP   |  53 \(DNS\)  |  For communication between a Storage Gateway VM and the DNS server\.  | 
|  Storage Gateway VM  |  AWS  |  TCP  |  22 \(Support channel\)  |  Allows AWS Support to access your gateway to help you with troubleshooting gateway issues\. You don't need this port open for the normal operation of your gateway, but it is required for troubleshooting\.  | 
|  Storage Gateway VM  |  Network Time Protocol \(NTP\) server  |  UDP  |  123 \(NTP\)  |  Used by local systems to synchronize VM time to the host time\. A Storage Gateway VM is configured to use the following NTP servers: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/Resource_Ports.html)  | 
|  Storage Gateway Hardware Appliance  |  Hypertext Transfer Protocol \(HTTP\) proxy  |  TCP  |  8080 \(HTTP\)  | Required briefly for activation\. | 

In addition to the common ports, volume and tape gateways also require the following ports\.


|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  iSCSI initiators  |  Storage Gateway VM  |  TCP  |  3260 \(iSCSI\)  |  By local systems to connect to iSCSI targets exposed by a gateway\.   | 