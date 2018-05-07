# Requirements<a name="Requirements"></a>

Unless otherwise noted, the following requirements are common to all gateway configurations\.

**Topics**
+ [Hardware and Storage Requirements](#requirements-hardware-storage)
+ [Network and Firewall Requirements](#networks)
+ [Supported Hypervisors and Host Requirements](#requirements-host)
+ [Supported NFS Clients for a File Gateway](#requirements-nfs-clients)
+ [Supported File System Operations for a File Gateway](#requirements-file-operations)
+ [Supported iSCSI Initiators](#requirements-iscsi-initiators)
+ [Supported Third\-Party Backup Applications for a Tape Gateway](#requirements-backup-sw-for-vtl)

## Hardware and Storage Requirements<a name="requirements-hardware-storage"></a>

In this section, you can find information about the minimum hardware and settings for your gateway and the minimum amount of disk space to allocate for the required storage\.

### Hardware Requirements<a name="requirements-hardware"></a>

When deploying your gateway on\-premises, you must make sure that the underlying hardware on which you deploy the gateway VM can dedicate the following minimum resources:
+ Four virtual processors assigned to the VM\. 
+ 16 GiB of reserved RAM assigned to the VM\. 
+ 80 GiB of disk space for installation of VM image and system data\. 

For more information, see [Optimizing Gateway Performance](Optimizing-common.md)\. For information about how your hardware affects the performance of the gateway VM, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\. 

### Amazon EC2 Instance Type Requirements<a name="requirements-hardware-ec2"></a>

When deploying your gateway on Amazon EC2, the instance size must be at least **xlarge** for your gateway to function\. However, for the compute\-optimized instance family the size must be at least

**2xlarge**\. Use one of the following instance types recommended for your gateway type\.

**Recommended for file gateway types**
+ General\-purpose instance family— m4 or m5 instance type\.
+ Compute\-optimized instance family— c4 or c5 instance types\. Select the **2xlarge** instance size or higher to meet the required RAM requirements\.
+ Memory\-optimized instance family—r3 instance types\.

**Recommended for cached volumes and tape gateway types**
+ General\-purpose instance family—m4 instance types\. We don't recommend using the **m4\.16xlarge** instance type\.
+ Compute\-optimized instance family—c4 instance types\. Select the **2xlarge** instance size or higher to meet the required RAM requirements\.
+ Storage\-optimized instance family—d2 or i2 instance types

**Note**  
When deploying your gateway on an Amazon EC2 instance, you must make sure that you allocate the following minimum resources:  
If you have more than 5 million objects in your S3 bucket and you are using a General Purposes SSD volume, a minimum root EBS volume of 350 GiB is needed for acceptable performance of your gateway during start up\. For information about how to increase your volume size, see [Modifying an EBS Volume from the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/console-modify.html)\.

### Storage Requirements<a name="requirements-storage"></a>

In addition to 80 GiB disk space for the VM, you also need additional disks for your gateway\.

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Upload Buffer \(Minimum\) | Upload Buffer \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | --- | --- | 
| File gateway | 150 GiB | 16 TiB | — | — | — | 
| Cached volume gateway | 150 GiB | 16 TiB | 150 GiB |  2 TiB  | — | 
| Stored volume gateway | — | — | 150 GiB |  2 TiB  | 1 or more for stored volume or volumes | 
| Tape gateway | 150 GiB | 16 TiB | 150 GiB | 2 TiB | — | 

**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.

For information about gateway limits, see [AWS Storage Gateway Limits](resource-gateway-limits.md)\.

## Network and Firewall Requirements<a name="networks"></a>

Your locally deployed gateway requires access to the internet, local networks, Domain Name Service \(DNS\) servers, firewalls, routers, and so on\. Following, you can find information about required ports and how to allow access through firewalls and routers\.

**Topics**
+ [Port Requirements](#requirements-network)
+ [Allowing AWS Storage Gateway Access Through Firewalls and Routers](#allow-firewall-gateway-access)
+ [Configuring Security Groups for Your Amazon EC2 Gateway Instance](#EC2GatewayCustomSecurityGroup-common)

### Port Requirements<a name="requirements-network"></a>

AWS Storage Gateway requires certain ports to be allowed for its operation\. The following illustrations show the required ports that you must allow for each type of gateway\. Some ports are required by all gateway types, and others are required by specific gateway types\. For more information about port requirements, see [Port Requirements](Resource_Ports.md)\.

**File Gateway**

The following illustration shows the ports to open for a file gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGWNetworkPorts16-file2.png)

**Volume Gateway and Tape Gateway**

The following illustration shows the ports to open for volume and tape gateways\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/SGWNetworkPorts16-volume-tape2.png)

### Allowing AWS Storage Gateway Access Through Firewalls and Routers<a name="allow-firewall-gateway-access"></a>

Your locally deployed gateway requires access to the following endpoints to communicate with AWS\. If you use a firewall or router to filter or limit network traffic, you must configure your firewall and router to allow these service endpoints for outbound communication to AWS\.

The following service endpoints are required by all gateways for control path \(anon\-cp, client\-cp, proxy\-app\) and data path \(dp\-1\) operations\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
```

The following service endpoint is required to make API calls\.

```
storagegateway.region.amazonaws.com:443
```

The Amazon S3 service endpoint, shown following, is used by file gateway only\. A file gateway requires this endpoint to access the S3 bucket that a file share maps to\.

If the gateway is not able to determine the AWS Region where your S3 bucket is located, this endpoint defaults to us\-east\-1\.s3\.amazonaws\.com\. We recommend you that whitelist the us\-east\-1 region in addition to AWS Regions where your gateway is activated, and where your S3 bucket is located\.

```
region.s3.amazonaws.com
```

The Amazon CloudFront endpoint following is required for Storage Gateway to get the list of available AWS Regions\.

```
https://d4kdq0yaxexbo.cloudfront.net/
```

The Storage Gateway VM is configured to use the following ntp servers:

```
0.amazon.pool.ntp.org
1.amazon.pool.ntp.org
2.amazon.pool.ntp.org
3.amazon.pool.ntp.org
```

 The following table provides a list of region strings for the available AWS Regions\.


| Region Name | Region String | File Gateway | Volume Gateway | Tape Gateway | 
| --- | --- | --- | --- | --- | 
| US East \(Ohio\) | us\-east\-2 | yes | yes | yes | 
| US East \(N\. Virginia\) | us\-east\-1 | yes | yes | yes | 
| US West \(N\. California\) | us\-west\-1 | yes | yes | yes | 
| US West \(Oregon\) | us\-west\-2 | yes | yes | yes | 
| Canada \(Central\) | ca\-central\-1 | yes | yes | yes | 
| EU \(Ireland\) | eu\-west\-1 | yes | yes | yes | 
| EU \(Frankfurt\) | eu\-central\-1 | yes | yes | yes | 
| EU \(London\) | eu\-west\-2 | yes | yes | yes | 
| EU \(Paris\) | eu\-west\-3 | yes | yes | yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | yes | yes | yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | yes | yes | yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | yes | yes | yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | yes | yes | yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | yes | yes | yes | 
| South America \(São Paulo\) | sa\-east\-1 | yes | yes | no | 

Depending on your gateway's AWS Region, replace *region* in the endpoint with the corresponding region string\. For example, if you create a gateway in the US West \(Oregon\) region, the endpoint looks like this: `storagegateway.us-west-2.amazonaws.com:443`\.

### Configuring Security Groups for Your Amazon EC2 Gateway Instance<a name="EC2GatewayCustomSecurityGroup-common"></a>

A security group controls traffic to your Amazon EC2 gateway instance\. When you create an instance from the Amazon Machine Image \(AMI\) for AWS Storage Gateway from AWS Marketplace, you have two choices for launching the instance\. To launch the instance by using the **1\-Click Launch** feature of AWS Marketplace, follow the steps in [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md) \. We recommend that you use this **1\-Click Launch** feature\. 

You can also launch an instance by using the **Manual Launch** feature in AWS Marketplace\. In this case, an autogenerated security group that is named `AWS Storage Gateway-1-0-AutogenByAWSMP-` is created\. This security group has the correct rule for port 80 to activate your gateway\. For more information about security groups, see [Security Group Concepts](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#concepts-security) in the *Amazon EC2 User Guide for Linux Instances*\.

Regardless of the security group that you use, we recommend the following:
+ The security group should not allow incoming connections from the outside internet\. It should allow only instances within the gateway security group to communicate with the gateway\. If you need to allow instances to connect to the gateway from outside its security group, we recommend that you allow connections only on ports 3260 \(for iSCSI connections\) and 80 \(for activation\)\.
+ If you want to activate your gateway from an EC2 host outside the gateway security group, allow incoming connections on port 80 from the IP address of that host\. If you cannot determine the activating host's IP address, you can open port 80, activate your gateway, and then close access on port 80 after completing activation\. 
+ Allow port 22 access only if you are using AWS Support for troubleshooting purposes\. For more information, see [Enabling AWS Support To Help Troubleshoot Your Gateway Hosted on an Amazon EC2 Instance](EC2GatewayTroubleshooting.md#EC2-EnableAWSSupportAccess)\.

In some cases, you might use an Amazon EC2 instance as an initiator \(that is, to connect to iSCSI targets on a gateway that you deployed on Amazon EC2\)\. In such a case, we recommend a two\-step approach: 

1. You should launch the initiator instance in the same security group as your gateway\.

1. You should configure access so the initiator can communicate with your gateway\.

For information about the ports to open for your gateway, see [Port Requirements](Resource_Ports.md)\.

## Supported Hypervisors and Host Requirements<a name="requirements-host"></a>

You can run AWS Storage Gateway either on\-premises as a virtual machine \(VM\) appliance, or in AWS as an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

AWS Storage Gateway supports the following hypervisor versions and hosts:
+ VMware ESXi Hypervisor \(version 4\.1, 5\.0, 5\.1, 5\.5, 6\.0 or 6\.5\)—A free version of VMware is available on the [VMware website](http://www.vmware.com/products/vsphere-hypervisor/overview.html)\. For this setup, you also need a VMware vSphere client to connect to the host\. 
+  Microsoft Hyper\-V Hypervisor \(version 2008 R2, 2012, or 2012 R2\)—A free, standalone version of Hyper\-V is available at the [Microsoft Download Center](http://www.microsoft.com/en-us/search/Results.aspx?q=hyper-V&form=DLC)\. For this setup, you need a Microsoft Hyper\-V Manager on a Microsoft Windows client computer to connect to the host\. 
+ EC2 instance—AWS Storage Gateway provides an Amazon Machine Image \(AMI\) that contains the gateway VM image\. Only file, cached volume, and tape gateway types can be deployed on Amazon EC2\. For information about how to deploy a gateway on Amazon EC2, see [Deploying a Volume or Tape Gateway on an Amazon EC2 Host](ec2-gateway-common.md)\.

**Note**  
AWS Storage Gateway doesn’t support recovering a gateway from a VM that was created from a snapshot or clone of another gateway VM or from your Amazon EC2 AMI\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway\. For more information, see [Recovering from an Unexpected Virtual Machine Shutdown](recover-data-from-gateway.md#recover-from-gateway-shutdown)\.

## Supported NFS Clients for a File Gateway<a name="requirements-nfs-clients"></a>

File gateways support the following NFS clients:
+ Amazon Linux 
+ Mac OS X
+ RHEL 7
+ SUSE Linux Enterprise Server 11 and SUSE Linux Enterprise Server 12
+ Ubuntu 14\.04 
+ Microsoft Windows 10 Enterprise, Windows Server 2012, and Windows Server 2016\. Native clients only support NFS version 3\.
+ Windows 7 Enterprise and Windows Server 2008\. 

  Native clients only support NFS v3\. The maximum supported NFS I/O size is 32 KB, so you might experience degraded performance on these versions of Windows\.

## Supported File System Operations for a File Gateway<a name="requirements-file-operations"></a>

Your NFS client can write, read, delete, and truncate files\. Writes are sent to Amazon S3 through optimized multipart uploads by using a write\-back cache\. Reads are first served through the local cache\. If data is not available, it's fetched through S3 as a read\-through cache\. 

Writes and reads are optimized in that only the parts that are changed or requested are transferred through your gateway\. Deletes remove objects from S3\. Directories are managed as folder objects in S3, using the same syntax as in the Amazon S3 Management Console\. 

## Supported iSCSI Initiators<a name="requirements-iscsi-initiators"></a>

When you deploy a cached volume or stored volume gateway, you can create iSCSI storage volumes on your gateway\. When you deploy a tape gateway, the gateway is preconfigured with one media changer and 10 tape drives\. These tape drives and the media changer are available to your existing client backup applications as iSCSI devices\. 

To connect to these iSCSI devices, AWS Storage Gateway supports the following iSCSI initiators:
+ Windows Server 2012 and Windows Server 2012 R2
+ Windows Server 2008 and Windows Server 2008 R2
+ Windows 7
+  Red Hat Enterprise Linux 5 
+  Red Hat Enterprise Linux 6 
+  Red Hat Enterprise Linux 7 
+  VMware ESX Initiator, which provides an alternative to using initiators in the guest operating systems of your VMs

**Important**  
Storage Gateway doesn't support Microsoft Multipath I/O \(MPIO\) from Windows clients\.  
Storage Gateway supports connecting multiple hosts to the same volume if the hosts coordinate access by using Windows Server Failover Clustering \(WSFC\)\. However, you can't connect multiple hosts to that same volume \(for example, sharing a nonclustered NTFS/ext4 file system\) without using WSFC\.

## Supported Third\-Party Backup Applications for a Tape Gateway<a name="requirements-backup-sw-for-vtl"></a>

You use a backup application to read, write, and manage tapes with a tape gateway\. The following third\-party backup applications are supported to work with tape gateways\.

The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with tape gateways\. This table includes the medium changer type recommended for each backup application\.


| Backup Application | Medium Changer Type | 
| --- | --- | 
| Arcserve Backup | AWS\-Gateway\-VTL | 
| Backup Exec 2012 | STK\-L700 | 
| Backup Exec 2014 | AWS\-Gateway\-VTL | 
| Backup Exec 15 | AWS\-Gateway\-VTL | 
| Backup Exec 16 | AWS\-Gateway\-VTL | 
| Commvault V11 | STK\-L700 | 
| Quest NetVault Backup 10\.0 | STK\-L700 | 
| Dell EMC NetWorker V8\.x or V9\.x | AWS\-Gateway\-VTL | 
| Micro Focus \(HPE\) Data Protector 9\.x | AWS\-Gateway\-VTL | 
| Microsoft System Center 2012 R2 Data Protection Manager Data Protection Manager doesn't display barcodes for virtual tapes created in AWS Storage Gateway\.  | STK\-L700 | 
| Symantec NetBackup Version 7\.x | AWS\-Gateway\-VTL | 
| Veeam Backup & Replication V7 | STK\-L700 | 
| Veeam Backup & Replication V8 | STK\-L700 | 
| Veeam Backup & Replication V9 Update 2 or later | AWS\-Gateway\-VTL | 

**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](resource_vtl-devices.md#change-mediumchanger-vtl)\.