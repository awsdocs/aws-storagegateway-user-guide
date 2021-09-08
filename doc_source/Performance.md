# Performance<a name="Performance"></a>

In this section, you can find information about Storage Gateway performance\.

**Topics**
+ [Performance guidance for file gateways](#performance-fgw)
+ [Optimizing Gateway Performance](#Optimizing-common)
+ [Using VMware vSphere High Availability with Storage Gateway](#vmware-ha)

## Performance guidance for file gateways<a name="performance-fgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your file gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\.

For best performance, the cache disk size must be tuned to the size of the active working set\. Using multiple local disks for the cache increases write performance by parallelizing access to data and leads to higher IOPS\.

In the following tables, *cache hit* read operations are reads from the file shares that are served from cache\. *Cache miss* read operations are reads from the file shares that are served from Amazon S3\.

**Note**  
We don't recommend using ephemeral storage\. For information about using ephemeral storage, see [Using ephemeral storage with EC2 gateways](ManagingLocalStorage-common.md#ephemeral-disk-cache)\.

Following are example file gateway configurations\.

### S3 File performance on Linux clients<a name="performance-fgw-linux-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/Performance.html)

### File gateway performance on Windows clients<a name="performance-fgw-windows-clients"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/files3/Performance.html)

**Note**  
Your performance might vary based on your host platform configuration and network bandwidth\. 

## Optimizing Gateway Performance<a name="Optimizing-common"></a>

You can find information following about how to optimize the performance of your gateway\. The guidance is based on adding resources to your gateway and adding resources to your application server\. 

### Add Resources to Your Gateway<a name="Optimizing-vtl-add-resources-common"></a>

 You can optimize gateway performance by adding resources to your gateway in one or more of the following ways\.

**Use higher\-performance disks**  
To optimize gateway performance, you can add high\-performance disks such as solid\-state drives \(SSDs\) and a NVMe controller\. You can also attach virtual disks to your VM directly from a storage area network \(SAN\) instead of the Microsoft Hyper\-V NTFS\. Improved disk performance generally results in better throughput and more input/output operations per second \(IOPS\)\. For information about adding disks, see [Adding cache storage](ManagingLocalStorage-common.md#ConfiguringLocalDiskStorage)\.  
To measure throughput, use the `ReadBytes` and `WriteBytes` metrics with the `Samples` Amazon CloudWatch statistic\. For example, the `Samples` statistic of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the IOPS\. As a general rule, when you review these metrics for a gateway, look for low throughput and low IOPS trends to indicate disk\-related bottlenecks\.   
CloudWatch metrics are not available for all gateways\. For information about gateway metrics, see [Monitoring your file gateway](monitoring-file-gateway.md)\.

**Add CPU resources to your gateway host**  
The minimum requirement for a gateway host server is four virtual processors\. To optimize gateway performance, confirm that the four virtual processors that are assigned to the gateway VM are backed by four cores\. In addition, confirm that you are not oversubscribing the CPUs of the host server\.   
When you add additional CPUs to your gateway host server, you increase the processing capability of the gateway\. Doing this allows your gateway to deal with, in parallel, both storing data from your application to your local storage and uploading this data to Amazon S3\. Additional CPUs also help ensure that your gateway gets enough CPU resources when the host is shared with other VMs\. Providing enough CPU resources has the general effect of improving throughput\.  
Storage Gateway supports using 24 CPUs in your gateway host server\. You can use 24 CPUs to significantly improve the performance of your gateway\. We recommend the following gateway configuration for your gateway host server:  
+ 24 CPUs\. 
+ 16 GiB of reserved RAM for file gateways
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
When you provision gateway disks, we strongly recommend that you don't provision local disks for local storage that use the same underlying physical storage disk\. For example, for VMware ESXi, the underlying physical storage resources are represented as a data store\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a virtual disk \(for example, as an upload buffer\), you can store the virtual disk in the same data store as the VM or a different data store\.   
If you have more than one data store, then we strongly recommend that you choose one data store for each type of local storage you are creating\. A data store that is backed by only one underlying physical disk can lead to poor performance\. An example is when you use such a disk to back both the cache storage and upload buffer in a gateway setup\. Similarly, a data store that is backed by a less high\-performing RAID configuration such as RAID 1 can lead to poor performance\.

### Add Resources to Your Application Environment<a name="Optimizing-vtl-add-resources-app-common"></a>

**Increase the bandwidth between your application server and your gateway**  
To optimize gateway performance, ensure that the network bandwidth between your application and the gateway can sustain your application needs\. You can use the `ReadBytes` and `WriteBytes` metrics of the gateway to measure the total data throughput\.   
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
![\[Editing cluster settings\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/edit-cluster-settings.png)

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
  + File gateway – 

### Deploy the Gateway<a name="vmware-ha-deploy-gateway"></a>

In your configured cluster, deploy the \.ova image to one of the cluster's hosts\.

**To deploy the gateway \.ova image**

1.  Deploy the \.ova image to one of the hosts in the cluster\.

1. Make sure the data stores that you choose for the root disk and the cache are available to all hosts in the cluster\.

### \(Optional\) Add Override Options for Other VMs on Your Cluster<a name="vmware-ha-overrides"></a>

If you have other VMs running on your cluster, you might want to set the cluster values specifically for each VM\. 

**To add override options for other VMs on your cluster**

1. On the **Summary** page in VMware vSphere, choose your cluster to open the cluster page, and then choose **Configure**\.

1. Choose the **Configuration** tab, and then choose **VM Overrides**\.

1. Add a new VM override option to change each value\. 

   For override options, see the following screenshot\.  
![\[Override cluster settings\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/override-cluster-settings.png)

### Activate Your Gateway<a name="vmware-ha-activate-gateway"></a>

After the \.ova for your gateway is deployed, activate your gateway\. The instructions about how are different for each gateway type\.

**To activate your gateway**
+  Choose activation instructions based on your gateway type:
  + File gateway – 

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

You can find information about VMware HA events in the Amazon CloudWatch log groups\. For more information, see [Getting file gateway health logs with CloudWatch log groups](monitoring-file-gateway.md#cw-log-groups)\.