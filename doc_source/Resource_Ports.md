# Port Requirements<a name="Resource_Ports"></a>

AWS Storage Gateway requires the following ports for its operation\. Some ports are common to all gateway types and are required by all gateways types\. Other ports are required by specific gateway types\. This section shows an illustration of the required ports and lists the ports required by each gateway type\.

**File Gateway**

The following illustration shows the ports to open for file gateway's operation\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGWNetworkPorts16-file2.png)

**Volume Gateways and Tape Gateway**

The following illustration shows the ports to open for volume gateways and tape gateway's operation\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGWNetworkPorts16-volume-tape2.png)

The following ports are common to all gateway types and are required by all gateway types\.


**Ports required by all gateway types**  

|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  Storage Gateway VM  |  AWS  |  TCP  |  443 \(HTTPS\)  |  For communication from AWS Storage Gateway VM to the AWS service endpoint\. For information about service endpoints, see [Allowing AWS Storage Gateway Access Through Firewalls and Routers](Requirements.md#allow-firewall-gateway-access)\.  | 
|  Your Web browser  |  Storage Gateway VM  |  TCP  |  80 \(HTTP\)  |  By local systems to obtain the storage gateway activation key\. Port 80 is only used during activation of the Storage Gateway appliance\.  AWS Storage Gateway VM does not require port 80 to be publicly accessible\. The required level of access to port 80 depends on your network configuration\. If you activate your gateway from the AWS Storage Gateway Management Console, the host from which you connect to the console must have access to your gateway’s port 80\.  | 
|  Storage Gateway VM  |  Domain Name Service \(DNS\) server  |  UDP/UDP  |  53 \(DNS\)  |  For communication between AWS Storage Gateway VM and the DNS server\.  | 
|  Storage Gateway VM  |  AWS  |  TCP  |  22 \(Support channel\)  |  Allows AWS Support to access your gateway to help you with troubleshooting gateway issues\. You don't need this port open for the normal operation of your gateway, but it is required for troubleshooting\.  | 
|  Storage Gateway VM  |  NTP server  |  UDP  |  123 \(NTP\)  |  Used by local systems to synchronize VM time to the host time\. The Storage Gateway VM is configured to use the following ntp servers: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/Resource_Ports.html)  | 

In addition to the common ports, file gateway requires the following ports\. 


**Ports required by file gateway only**  

|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  NFS client  |  Storage Gateway VM  |  TCP/UDP  |  2049 \(NFS\)  |  For local systems to connect to NFS shares your gateway exposes\.  | 
|  NFS client  |  Storage Gateway VM  |  TCP/UDP  |  111 \(NFS\)  |  For local systems to connect to the portmapper your gateway exposes\.  Only needed for NFSv3 clients\.  | 
|  NFS client  |  Storage Gateway VM  |  TCP/UDP  |  20048 \(NFS\)  |  For local systems to connect to mountd your gateway exposes\.  Only needed for NFSv3 clients\.  | 

In addition to the common ports, volume and tape gateways require the following port\.


|  From  |  To  |  Protocol  |  Port  |  How Used  | 
| --- | --- | --- | --- | --- | 
|  iSCSI Initiators  |  Storage Gateway VM  |  TCP  |  3260 \(iSCSI\)  |  By local systems to connect to iSCSI targets exposed by the gateway\.   | 