# Performance<a name="Performance"></a>

In this section, you can find information about AWS Storage Gateway performance\.

**Topics**
+ [Performance Guidance for File Gateways](#performance-fgw)
+ [Performance Guidance for Tape Gateways](#performance-tgw)
+ [Optimizing Gateway Performance](#Optimizing-common)

## Performance Guidance for File Gateways<a name="performance-fgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your file gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\.

**Cache Disk Configuration**  
For best performance, the cache disk size must be tuned to the size of the active working set\. Using multiple local disks for the cache increases write performance by parallelizing access to data and leads to higher IOPS\.

Following are recommended configurations for your file gateway\. 


| Recommended Configuration  | Write Throughput \(File Sizes > 6 MB\) | 
| --- | --- | 
|  Root disk: 80 GB io1, 4,000 IOPS Cache disk: 512 GiB EBS cache, io1, 1,500 provisioned IOPS  Minimum network performance: 1 Gbps Amazon EC2 instance: c5\.4xlarge  | 125 MiB/s \(0\.9 Gbps\) | 
| [Storage Gateway Hardware Appliance](https://www.amazon.com/dp/B079RBVX3M) Minimum network performance: 5 Gbps  | 300 MiB/s \(2\.3 Gbps\) | 
|  Root disk: 80 GB io1, 4,000 IOPS Cache disk: Two 1\.9 TiB NVME caches \(ephemeral\) Minimum network performance: 5 Gbps Amazon EC2 instance: i3\.4xlarge \([Using Ephemeral Storage With EC2 Gateways](ManagingLocalStorage-common.md#ephemral-disk-cache)\)   | 500 MiB/s \(3\.9 Gbps\) | 

## Performance Guidance for Tape Gateways<a name="performance-tgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your tape gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\.


| Configuration | Read/Write from/to cache | Read/Write from/to Cloud | 
| --- | --- | --- | 
|  | Write Gbps  | Read Gbps  | Write Gbps  | Read Gbps  | 
|  Host Platform: Amazon EC2 instance—c5\.4xlarge  Cache disk: 150 GB Upload buffer: 150 GB CPU: 16vCPU \| RAM: 32 GB Minimum network performance: 10 Gbps  | 2\.3  | 3\.2  | 1\.2  | 0\.6  | 
|  Host Platform: [Storage Gateway Hardware Appliance](https://www.amazon.com/dp/B079RBVX3M) Cache disk: 2\.5 TB Upload buffer: 2 TB CPU: 20 cores \| RAM: 128 GB Minimum network performance: 10 Gbps  | 1\.4  | 4\.3  | 1\.4  | 0\.5  | 
|  Host Platform: Amazon EC2instance—c5d\.9xlarge Cache disk: 450 GB NVMe Upload buffer: 450 GB NVMe CPU: 36 vCPU \| RAM: 72 GB Minimum network performance: 10Gbps  | 2\.7  | 3\.9  | 1\.3  | 0\.7  | 

**Note**  
This performance was achieved by using1 MB block size and 4 tape drives simultaneously\. 

## Optimizing Gateway Performance<a name="Optimizing-common"></a>

You can find information following about how to optimize the performance of your gateway\. The guidance is based on adding resources to your gateway and adding resources to your application server\. 

### Add Resources to Your Gateway<a name="Optimizing-vtl-add-resources-common"></a>

**Use higher\-performance disks**  
To optimize gateway performance, you can add high performance disks such as solid\-state drives \(SSDs\) and a NVMe controller\. You can also attach virtual disks to your VM directly from a storage area network \(SAN\) instead of the Microsoft Hyper\-V NTFS\. Improved disk performance generally results in better throughput and more input/output operations per second \(IOPS\)\. To measure throughput, use the `ReadBytes` and `WriteBytes` metrics with the `Samples` Amazon CloudWatch statistic\. For example, the `Samples` statistic of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the IOPS\. As a general rule, when you review these metrics for a gateway, look for low throughput and low IOPS trends to indicate disk\-related bottlenecks\. For more information about gateway metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\.   
CloudWatch metrics are not available for all gateways\. For information about gateway metrics, see [Monitoring Your Gateway and Resources](Main_monitoring-gateways-common.md)

**Add CPU resources to your gateway host**  
The minimum requirement for a gateway host server is four virtual processors\. To optimize gateway performance, you should confirm that the four virtual processors that are assigned to the gateway VM are backed by four cores and that you are not oversubscribing the CPUs of the host server\. When you add additional CPUs to your gateway host server, you increase the processing capability of the gateway to deal with, in parallel, both storing data from your application to your local storage and uploading this data to Amazon S3\. Additional CPUs also help ensure that your gateway gets enough CPU resources when the host is shared with other VMs\. Providing enough CPU resources has the general effect of improving throughput\.  
AWS Storage Gateway supports using 24 CPUs in your gateway host server\. You can use 24 CPUs to significantly improve the performance of your gateway\. We recommend the following gateway configuration for your gateway host server:  
+ 24 CPUs 
+ 16 GiB of reserved RAM
+ Disk 1 attached to paravirtual controller 1, to be used as the gateway cache as follows:
  + SSD using an NVMe controller 
+ Disk 2 attached to paravirtual controller 1, to be used as the gateway upload buffer as follows:
  + SSD using an NVMe controller
+ Disk 3 attached to paravirtual controller 2, to be used as the gateway upload buffer as follows:
  + SSD using an NVMe controller
+ Network adapter 1 configured on VM network 1:
  + Use VM network 1 and add VMXnet3 \(10 Gbps\) to be used for ingestion
+ Network adapter 2 configured on VM network 2:
  + Use VM network 2 and add a VMXnet3 \(10 Gbps\) to be used to connect to AWS

 **Back gateway virtual disks with separate physical disks**  
When you provision disks in a gateway setup, we strongly recommend that you do not provision local disks for the upload buffer and cache storage that use the same underlying physical storage disk\. For example, for VMware ESXi, the underlying physical storage resources are represented as a data store\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a virtual disk \(for example, to use as an upload buffer\), you have the option to store the virtual disk in the same data store as the VM or a different data store\. If you have more than one data store, then we strongly recommend that you choose one data store for each type of local storage you are creating\. A data store that is backed by only one underlying physical disk, or that is backed by a less\-performant RAID configuration such as RAID 1, can lead to poor performance—for example, when used to back both the cache storage and upload buffer in a gateway setup\.

**Change the volumes configuration**  
For volumes gateways, if you find that adding more volumes to a gateway reduces the throughput to the gateway, consider adding the volumes to a separate gateway\. In particular, if a volume is used for a high\-throughput application, consider creating a separate gateway for the high\-throughput application\. However, as a general rule, you should not use one gateway for all of your high\-throughput applications and another gateway for all of your low\-throughput applications\. To measure your volume throughput, use the `ReadBytes` and `WriteBytes` metrics\. For more information on these metrics, see [Measuring Performance Between Your Application and Gateway](GatewayMetrics-common.md#PerfAppGateway-common)\.

### Use a Larger Block Size for Tape Drives<a name="block-size"></a>

For tape gateway, the default block size for a tape drive is 64 KB but you can increase the block size up to 1 MB to improve I/O performance\. The size you choose depends on the block size limitations of your backup software\. We recommend setting the block size of the tape drives in the your backup software to either 128 KB or 256 KB or 512 KB or 1 MB\. For more information, see the documentation for your backup software\.

For more information about specific gateway performance guidance, see [Performance](#Performance)\.

### Add Resources to Your Application Environment<a name="Optimizing-vtl-add-resources-app-common"></a>

**Increase the bandwidth between your application server and your gateway**  
To optimize gateway performance, ensure that the network bandwidth between your application and the gateway can sustain your application needs\. You can use the `ReadBytes` and `WriteBytes` metrics of the gateway to measure the total data throughput \(for more information on these metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\)\. For your application, compare the measured throughput with the desired throughput\. If the measured throughput is less than the desired throughput, then increasing the bandwidth between your application and gateway can improve performance if the network is the bottleneck\. Similarly, you can increase the bandwidth between your VM and your local disks, if they're not direct\-attached\.

**Add CPU resources to your application environment**  
If your application can make use of additional CPU resources, then adding more CPUs can help your application to scale its I/O load\.