--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Synchronizing Your Gateway VM Time<a name="MaintenanceTimeSync-hyperv"></a>

For a gateway deployed on VMware ESXi, setting the hypervisor host time and synchronizing the VM time to the host is sufficient to avoid time drift\. For more information, see [Synchronizing VM Time with Host Time](configure-vmware.md#GettingStartedSyncVMTime-common)\. For a gateway deployed on Microsoft Hyper\-V, you should periodically check your VM's time using the procedure described following\. 

**To view and synchronize the time of a hypervisor gateway VM to a Network Time Protocol \(NTP\) server**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the local console for Linux Kernel\-based Virtuam Machine \(KVM\), see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. On the **Storage Gateway Configuration** main menu, enter **4** for **System Time Management**\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/LocalConsoleLogin.png)

1. On the **System Time Management** menu, enter **1** for **View and Synchronize System Time**\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync01.png)

1. If the result indicates that you should synchronize your VM's time to the NTP time, enter **y**\. Otherwise, enter **n**\.

   If you enter **y** to synchronize, the synchronization might take a few moments\.

   The following screenshot shows a VM that doesn't require time synchronization\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync02.png)

   The following screenshot shows a VM that does require time synchronization\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-timesync03.png)