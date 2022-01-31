--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Performance<a name="Performance"></a>

In this section, you can find information about Storage Gateway performance\.

**Topics**
+ [Performance guidance for File Gateways](#performance-fgw)
+ [Performance guidance for tape gateways](#performance-tgw)
+ [Optimizing Gateway Performance](#Optimizing-common)
+ [Using VMware vSphere High Availability with Storage Gateway](#vmware-ha)

## Performance guidance for File Gateways<a name="performance-fgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your file gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\.

Following are example file gateway configurations\.

### File gateway performance on Linux clients<a name="performance-fgw-linux-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/Performance.html)

### File gateway performance on Windows clients<a name="performance-fgw-linux-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/Performance.html)

**Note**  
Your performance might vary based on your host platform configuration and network bandwidth\. 

## Performance guidance for tape gateways<a name="performance-tgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your tape gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\.
+ File Gateway is an Object Store cache\. File Gateway performance characteristics will differ from those of file servers\.
+ File Gateway scales best when the workload is multi\-threaded \(or involves multiples clients\)\. Many basic file management tools are single\-threaded, including Windows Explorer, the copy or cp commands, and robocopy without the /mt switch\. These tools will not be able to fully utilize File Gateway's scalability\. 
+ For best performance results, you should scale your workloads by adding threads and/or clients\.
+ The highest throughput, MiB/sec, is achieved when transferring large files \(tens or hundreds of MiB each\) using multiple threads\.
+ Due to the overhead of new file creation, transferring many small files will result in a lower MiB/sec throughput than the same workload using large files\. 
+ To achieve best performance when transferring small files, you should use multiple threads and/or clients\. 
+ We recommend that you measure small file transfer rates in files\-per\-second, not MiB\-per\-second since file creation overhead dominates such a workload\.


| Configuration | Write Throughput Gbps | Read from Cache Throughput Gbps | Read from Amazon Web Services Cloud Throughput Gbps | 
| --- | --- | --- | --- | 
|  Host Platform: Amazon EC2 instance— c5\.4xlarge  Root disk: 80 GB, io1 SSD, 4000 IOPs Cache disk: 50 GB, io1 SSD, 2000 IOPs Upload buffer disk: 450 GB, io1 SSD, 2000 IOPs CPU: 16 vCPU \| RAM: 32 GB Network bandwidth to cloud: 10 Gbps  | 2\.3  | 4\.0  | 1\.7  | 
|  Host platform: [Storage Gateway Hardware Appliance](https://www.amazon.com/dp/B079RBVX3M) Cache disk: 2\.5 TB Upload buffer disk: 2 TB  CPU: 20 cores \| RAM: 128 GB  Network bandwidth to cloud: 10 Gbps   | 2\.3  | 4\.2  | 1\.4  | 
|  Host platform: Amazon EC2instance— c5d\.9xlarge  Root disk: 80 GB, io1 SSD, 4000 IOPs Cache disk: 900 GB NVMe disk Upload buffer disk: 900 GB NVMe disk  CPU: 36 vCPU \| RAM: 72 GB Network bandwidth to cloud: 10 Gbps  | 5\.2  | 8\.2  | 2\.0  | 

**Note**  
This performance was achieved by using a 1 MB block size and three tape drives simultaneously\.  
Your performance might vary based on your host platform configuration and network bandwidth\.

 To improve write and read throughput performance of your tape gateway, see [Optimize iSCSI Settings](#optimize-iSCSI), [Use a Larger Block Size for Tape Drives](#block-size), and [Optimize the Performance of Virtual Tape Drives in the Backup Software](#optimize-virtual-tape-drive)\. 

## Optimizing Gateway Performance<a name="Optimizing-common"></a>

You can find information following about how to optimize the performance of your gateway\. The guidance is based on adding resources to your gateway and adding resources to your application server\. 

### Add Resources to Your Gateway<a name="Optimizing-vtl-add-resources-common"></a>

 You can optimize gateway performance by adding resources to your gateway in one or more of the following ways\.

**Use higher\-performance disks**  
To optimize gateway performance, you can add high\-performance disks such as solid\-state drives \(SSDs\) and a NVMe controller\. You can also attach virtual disks to your VM directly from a storage area network \(SAN\) instead of the Microsoft Hyper\-V NTFS\. Improved disk performance generally results in better throughput and more input/output operations per second \(IOPS\)\.   
To measure throughput, use the `ReadBytes` and `WriteBytes` metrics with the `Samples` Amazon CloudWatch statistic\. For example, the `Samples` statistic of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the IOPS\. As a general rule, when you review these metrics for a gateway, look for low throughput and low IOPS trends to indicate disk\-related bottlenecks\. For more information about gateway metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\.   
CloudWatch metrics are not available for all gateways\. For information about gateway metrics, see [Monitoring Storage Gateway](Main_monitoring-gateways-common.md)\.

**Add CPU resources to your gateway host**  
The minimum requirement for a gateway host server is four virtual processors\. To optimize gateway performance, confirm that the four virtual processors that are assigned to the gateway VM are backed by four cores\. In addition, confirm that you are not oversubscribing the CPUs of the host server\.   
When you add additional CPUs to your gateway host server, you increase the processing capability of the gateway\. Doing this allows your gateway to deal with, in parallel, both storing data from your application to your local storage and uploading this data to Amazon S3\. Additional CPUs also help ensure that your gateway gets enough CPU resources when the host is shared with other VMs\. Providing enough CPU resources has the general effect of improving throughput\.  
Storage Gateway supports using 24 CPUs in your gateway host server\. You can use 24 CPUs to significantly improve the performance of your gateway\. We recommend the following gateway configuration for your gateway host server:  
+ 24 CPUs\. 
+ 16 GiB of reserved RAM for file gateways

  For volume and tape gateways, your hardware should dedicate the following amounts of RAM:
  + 16 GiB of reserved RAM for gateways with cache size up to 16 TiB
  + 32 GiB of reserved RAM for gateways with cache size 16 TiB to 32 TiB
  + 48 GiB of reserved RAM for gateways with cache size 32 TiB to 64 TiB
+ Disk 1 attached to paravirtual controller 1, to be used as the gateway cache as follows:
  + SSD using an NVMe controller\. 
+ Disk 2 attached to paravirtual controller 1, to be used as the gateway upload buffer as follows:
  + SSD using an NVMe controller\.
+ Disk 3 attached to paravirtual controller 2, to be used as the gateway upload buffer as follows:
  + SSD using an NVMe controller\.
+ Network adapter 1 configured on VM network 1:
  + Use VM network 1 and add VMXnet3 \(10 Gbps\) to be used for ingestion\.
+ Network adapter 2 configured on VM network 2:
  + Use VM network 2 and add a VMXnet3 \(10 Gbps\) to be used to connect to AWS\.

 **Back gateway virtual disks with separate physical disks**  
When you provision gateway disks, we strongly recommend that you don't provision local disks for the upload buffer and cache storage that use the same underlying physical storage disk\. For example, for VMware ESXi, the underlying physical storage resources are represented as a data store\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a virtual disk \(for example, as an upload buffer\), you can store the virtual disk in the same data store as the VM or a different data store\.   
If you have more than one data store, then we strongly recommend that you choose one data store for each type of local storage you are creating\. A data store that is backed by only one underlying physical disk can lead to poor performance\. An example is when you use such a disk to back both the cache storage and upload buffer in a gateway setup\. Similarly, a data store that is backed by a less high\-performing RAID configuration such as RAID 1 can lead to poor performance\.

**Change the volumes configuration**  
For volume gateways, if you find that adding more volumes to a gateway reduces the throughput to the gateway, consider adding the volumes to a separate gateway\. In particular, if a volume is used for a high\-throughput application, consider creating a separate gateway for the high\-throughput application\. However, as a general rule, you should not use one gateway for all of your high\-throughput applications and another gateway for all of your low\-throughput applications\. To measure your volume throughput, use the `ReadBytes` and `WriteBytes` metrics\.   
For more information about these metrics, see [Measuring Performance Between Your Application and Gateway](monitoring-volume-gateway.md#PerfAppGateway-common)\.

### Optimize iSCSI Settings<a name="optimize-iSCSI"></a>

 You can optimize iSCSI settings on your iSCSI initiator to achieve higher I/O performance\. We recommend choosing 256 KiB for `MaxReceiveDataSegmentLength` and `FirstBurstLength`, and 1 MiB for `MaxBurstLength`\. For more information about configuring iSCSI settings, see [Customizing iSCSI Settings](initiator-connection-common.md#recommendediSCSISettings)\. 

**Note**  
These recommended settings can enable overall better performance\. However, the specific iSCSI settings that are needed to optimize performance vary depending on which backup software you use\. For details, see your backup software documentation\. 

### Use a Larger Block Size for Tape Drives<a name="block-size"></a>

For a Tape Gateway, the default block size for a tape drive is 64 KB\. However, you can increase the block size up to 1 MB to improve I/O performance\. 

The block size that you choose depends on the maximum block size that your backup software supports\. We recommend that you set the block size of the tape drives in your backup software to a size that is as large as possible\. However, this block size must not be greater than the 1 MB maximum size that the gateway supports\. 

Tape gateways negotiate the block size for virtual tape drives to automatically match what is set on the backup software\. When you increase the block size on the backup software, we recommend that you also check the settings to ensure that the host initiator supports the new block size\. For more information, see the documentation for your backup software\. For more information about specific gateway performance guidance, see [Performance](#Performance)\.

### Optimize the Performance of Virtual Tape Drives in the Backup Software<a name="optimize-virtual-tape-drive"></a>

Your backup software can back up data on up to 10 virtual tape drives on a tape gateway at the same time\. We recommend that you configure backup jobs in your backup software to use at least 4 virtual tape drives simultaneous on the tape gateway\. You can achieve better write throughput when the backup software is backing up data to more than one virtual tape at the same time\. 

### Add Resources to Your Application Environment<a name="Optimizing-vtl-add-resources-app-common"></a>

**Increase the bandwidth between your application server and your gateway**  
To optimize gateway performance, ensure that the network bandwidth between your application and the gateway can sustain your application needs\. You can use the `ReadBytes` and `WriteBytes` metrics of the gateway to measure the total data throughput\. For more information about these metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\.   
For your application, compare the measured throughput with the desired throughput\. If the measured throughput is less than the desired throughput, then increasing the bandwidth between your application and gateway can improve performance if the network is the bottleneck\. Similarly, you can increase the bandwidth between your VM and your local disks, if they're not direct\-attached\.

**Add CPU resources to your application environment**  
If your application can use additional CPU resources, then adding more CPUs can help your application to scale its I/O load\.

## Using VMware vSphere High Availability with Storage Gateway<a name="vmware-ha"></a>

Storage Gateway provides high availability on VMware through a set of application\-level health checks integrated with VMware vSphere High Availability \(VMware HA\)\. This approach helps protect storage workloads against hardware, hypervisor, or network failures\. It also helps protect against software errors, such as connection timeouts and file share or volume unavailability\.

With this integration, a gateway deployed in a VMware environment on\-premises or in a VMware Cloud on AWS automatically recovers from most service interruptions\. It generally does this in under 60 seconds with no data loss\. 

To use VMware HA with Storage Gateway, take the steps listed following\.

**Topics**
+ [Configure Your vSphere VMware HA Cluster](#vmware-ha-configure-cluster)
+ [Download the \.ova Image for Your Gateway Type](#vmware-ha-download-image)
+ [Deploy the Gateway](#vmware-ha-deploy-gateway)
+ [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)
+ [Activate Your Gateway](#vmware-ha-activate-gateway)
+ [Test Your VMware High Availability Configuration](#vmware-ha-test-failover)

### Configure Your vSphere VMware HA Cluster<a name="vmware-ha-configure-cluster"></a>

First, if you haven’t already created a VMware cluster, create one\. For information about how to create a VMware cluster, see [Create a vSphere HA Cluster](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.avail.doc/GUID-4BC60283-B638-472F-B1D2-1E4E57EAD213.html) in the VMware documentation\.

Next, configure your VMware cluster to work with Storage Gateway\.

**To configure your VMware cluster**

1. On the **Edit Cluster Settings** page in VMware vSphere, make sure that VM monitoring is configured for VM and application monitoring\. To do so, set the following options as listed:
   + **Host Failure Response**: **Restart VMs**
   + **Response for Host Isolation**: **Shut down and restart VMs**
   + **Datastore with PDL**: **Disabled**
   + **Datastore with APD**: **Disabled**
   + **VM Monitoring**: **VM and Application Monitoring**

   For an example, see the following screenshot\.   
![\[Editing cluster settings\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/edit-cluster-settings.png)

1. Fine\-tune the sensitivity of the cluster by adjusting the following values: 
   + **Failure interval** – After this interval, the VM is restarted if a VM heartbeat isn't received\. 
   + **Minimum uptime** – The cluster waits this long after a VM starts to begin monitoring for VM tools' heartbeats\.
   + **Maximum per\-VM resets** – The cluster restarts the VM a maximum of this many times within the maximum resets time window\.
   + **Maximum resets time window** – The window of time in which to count the maximum resets per\-VM resets\. 

   If you aren't sure what values to set, use these example settings: 
   + **Failure interval**: **30** seconds 
   + **Minimum uptime**: **120** seconds 
   + **Maximum per\-VM resets**: **3**
   + **Maximum resets time window**: **1** hour 

If you have other VMs running on the cluster, you might want to set these values specifically for your VM\. You can't do this until you deploy the VM from the \.ova\. For more information on setting these values, see [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)\.

### Download the \.ova Image for Your Gateway Type<a name="vmware-ha-download-image"></a>

Use the following procedure to download the \.ova image\.

**To download the \.ova image for your gateway type**
+ Download the \.ova image for your gateway type from one of the following:
  + File gateway – [Creating a File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/create-file-gateway.html)
  + Volume gateway – [Creating a Gateway](create-volume-gateway.md)
  + Tape gateway – [Creating a Gateway](create-gateway-vtl.md)

### Deploy the Gateway<a name="vmware-ha-deploy-gateway"></a>

In your configured cluster, deploy the \.ova image to one of the cluster's hosts\.

**To deploy the gateway \.ova image**

1.  Deploy the \.ova image to one of the hosts in the cluster\.

1. Make sure the data stores that you choose for the root disk and the cache are available to all hosts in the cluster\.When deploying the storage gateway \.ova file in a VMware or on\-prem environment, the disks are described as paravirtualized SCSI disks\. If, however, you create using the pVSCSI adapter type for the disks in VMware, the AWS management console returns that it is unable to read the disks\. To address that, instead use the "LSI Logic SAS" adapter type for the disks so AWS management console actually sees the disks and is able to proceed for deployment\.

### \(Optional\) Add Override Options for Other VMs on Your Cluster<a name="vmware-ha-overrides"></a>

If you have other VMs running on your cluster, you might want to set the cluster values specifically for each VM\. 

**To add override options for other VMs on your cluster**

1. On the **Summary** page in VMware vSphere, choose your cluster to open the cluster page, and then choose **Configure**\.

1. Choose the **Configuration** tab, and then choose **VM Overrides**\.

1. Add a new VM override option to change each value\. 

   For override options, see the following screenshot\.  
![\[Override cluster settings\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/override-cluster-settings.png)

### Activate Your Gateway<a name="vmware-ha-activate-gateway"></a>

After the \.ova for your gateway is deployed, activate your gateway\. The instructions about how are different for each gateway type\.

**To activate your gateway**
+  Choose activation instructions based on your gateway type:
  + File gateway – [Creating a File Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/create-file-gateway.html)
  + Volume gateway – [Creating a Gateway](create-volume-gateway.md)
  + Tape gateway – [Creating a Gateway](create-gateway-vtl.md)

### Test Your VMware High Availability Configuration<a name="vmware-ha-test-failover"></a>

After you activate your gateway, test your configuration\.

**To test your VMware HA configuration**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and then choose the gateway that you want to test for VMware HA\.

1. For **Actions**, choose **Verify VMware HA**\.

1. In the **Verify VMware High Availability Configuration** box that appears, choose **OK**\.
**Note**  
Testing your VMware HA configuration reboots your gateway VM and interrupts connectivity to your gateway\. The test might take a few minutes to complete\. 

   If the test is successful, the status of **Verified** appears in the details tab of the gateway in the console\.

1. Choose **Exit**\.

You can find information about VMware HA events in the Amazon CloudWatch log groups\. For more information, see [Getting file gateway health logs with CloudWatch Log Groups](monitoring-file-gateway.md#cw-log-groups)\.