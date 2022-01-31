--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Moving your data to a new gateway<a name="migrate-data"></a>

You can move data between gateways as your data and performance needs grow, or if you receive an AWS notification to migrate your gateway\. The following are some reasons for doing this:
+ Move your data to better host platforms or newer Amazon EC2 instances\.
+ Refresh the underlying hardware for your server\.

The steps that you follow to move your data to a new gateway depend on the gateway type that you have\.

**Note**  
Data can only be moved between the same gateway types\.

**Topics**
+ [Migrating your Amazon S3 File Gateway](#migrate-data-file-gateway)
+ [Moving stored volumes to a new stored volume gateway](#migrate-data-volume-stored)
+ [Moving cached volumes to a new cached volume gateway virtual machine](#migrate-data-volume-cached)
+ [Moving virtual tapes to a new tape gateway](#migrate-data-tape)

## Migrating your Amazon S3 File Gateway<a name="migrate-data-file-gateway"></a>

If you receive an AWS notification to migrate your file gateway, see [Migrating your Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/migrate-data.html) in the *User Guide for Amazon S3 File Gateway*\.

## Moving stored volumes to a new stored volume gateway<a name="migrate-data-volume-stored"></a>



**To move your stored volume to a new stored volume gateway**

1. Stop any applications that are writing to the old stored volume gateway\.

1. Use the following steps to create a snapshot of your volume, and then wait for the snapshot to complete\.

   1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

   1. In the navigation pane, choose **Volumes**, and then choose the volume that you want to create the snapshot from\.

   1. For **Actions**, choose **Create snapshot**\.

   1. In the **Create snapshot** dialog box, enter a snapshot description, and then choose **Create snapshot**\.

      You can verify that the snapshot was created using the console\. If data is still uploading to the volume, wait until the upload is complete before you go to the next step\. To see the snapshot status and validate that none are pending, select the snapshot links on the volumes\.

1. Use the following steps to stop the old stored volume gateway:

   1. In the navigation pane, choose **Gateways**, and then choose the old stored volume gateway that you want to stop\. The status of the gateway is **Running**\.

   1. For **Actions**, choose **Stop gateway**\. Verify the ID of the gateway from the dialog box, and then choose **Stop gateway**\.

      While the gateway is stopping, you might see a message that indicates the status of the gateway\. When the gateway shuts down, a message and a **Start gateway** button appear in the **Details** tab\. When the gateway shuts down, the status of the gateway is **Shutdown**\.

   1. Shut down the VM using the hypervisor controls\.

   For more information about stopping a gateway, see [Starting and Stopping a Volume or Tape Gateway](MaintenanceShutDown-common.md#start-stop-classic)\.

1. Detach the storage disks associated with your stored volumes from the gateway VM\. This excludes the root disk of the VM\.

1. Activate a new stored volume gateway with a new hypervisor VM image available from the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Attach the physical storage disks that you detached from the old stored volume gateway VM in step 5\.

1. To preserve existing data on the disk, use the following steps to create stored volumes\.

   1. On the Storage Gateway console, choose **Create volume**\.

   1. In the **Create volume** dialog box, select the stored volume gateway that you created in step 5\.

   1. Choose a **Disk ID** value from the list\.

   1. For **Volume content**, select the **Preserve existing data on the disk** option\.

   For more information about creating volumes, see [Creating a volume](GettingStartedCreateVolumes.md)\.

1. \(Optional\) In the **Configure CHAP authentication** wizard that appears, enter the **Initiator name**, **Initiator secret**, and **Target secret**, and then choose **Save**\.

   For more information about working with Challenge\-Handshake Authentication Protocol \(CHAP\) authentication, see [Configuring CHAP Authentication for Your iSCSI Targets](initiator-connection-common.md#ConfiguringiSCSIClientInitiatorCHAP)\.

1. Start the application that writes to your stored volume\.

1. When you have confirmed that your new stored volume gateway is working correctly, you can delete the old stored volume gateway\.
**Important**  
Before you delete a gateway, be sure that no applications are currently writing to that gateway's volumes\. If you delete a gateway while it is in use, data loss can occur\.

   Use the following steps to delete the old stored volume gateway:
**Warning**  
When a gateway is deleted, there is no way to recover it\.

   1. In the navigation pane, choose **Gateways**, and then choose the old stored volume gateway that you want to delete\.

   1. For **Actions**, choose **Delete gateway**\.

   1. In the confirmation dialog box that appears, select the check box to confirm your deletion\. Make sure that the gateway ID listed specifies the old stored volume gateway that you want to delete, and then choose **Delete**\.  
![\[Console screenshot showing the confirmation dialog box with the check box selected.\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/delete-gateway.png)

1. Delete the old gateway VM\. For information about deleting a VM, see the documentation for your hypervisor\.

## Moving cached volumes to a new cached volume gateway virtual machine<a name="migrate-data-volume-cached"></a>

**To move your cached volumes to a new cached volume gateway virtual machine \(VM\)**

1. Stop any applications that are writing to the old cached volume gateway\.

1. Unmount or disconnect iSCSI volumes from any clients that are using them\. This helps keep data on those volumes consistent by preventing clients from changing or adding data to those volumes\.

1. Use the following steps to create a snapshot of your volume, and then wait for the snapshot to complete\.

   1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

   1. In the navigation pane, choose **Volumes**, and then choose the volume that you want to create the snapshot from\.

   1. For **Actions**, choose **Create snapshot**\.

   1. In the **Create snapshot** dialog box, enter a snapshot description, and then choose **Create snapshot**\.

      You can verify that the snapshot was created using the console\. If data is still uploading to the volume, wait until the upload is complete before you go to the next step\. To see the snapshot status and validate that none are pending, select the snapshot links on the volumes\.

      For more information about checking volume status in the console, see [Understanding Volume Statuses and Transitions](managing-volumes.md#StorageVolumeStatuses)\. For information about cached volume status, see [Understanding Cached Volume Status Transitions ](managing-volumes.md#CachedVolumeStatusTransition)\.

1. Use the following steps to stop the old cached volume gateway:

   1. In the navigation pane, choose **Gateways**, and then choose the old cached volume gateway that you want to stop\. The status of the gateway is **Running**\.

   1. For **Actions**, choose **Stop gateway**\. Verify the ID of the gateway from the dialog box, and then choose **Stop gateway**\. Make a note of the gateway ID, as it is needed in a later step\.

      While the old gateway is stopping, you might see a message that indicates the status of the gateway\. When the old gateway shuts down, a message and a **Start gateway** button appear in the **Details** tab\. When the gateway shuts down, the status of the gateway is **Shutdown**\.

   1. Shut down the old VM using the hypervisor controls\. For more information about shutting down an Amazon EC2 instance, see [Stopping and starting your instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Stop_Start.html#starting-stopping-instances) in the *Amazon EC2 User Guide for Windows Instances*\. For more information about shutting down a KVM, VMware, or Hyper\-V VM, see your hypervisor documentation\.

   For more information about stopping a gateway, see [Starting and Stopping a Volume or Tape Gateway](MaintenanceShutDown-common.md#start-stop-classic)\.

1. Detach all disks, including the root disk, cache disks, and upload buffer disks, from the old gateway VM\.
**Note**  
Make a note of the root disk's volume ID, as well as the gateway ID associated with that root disk\. You detach this disk from the new storage gateway hypervisor in a later step\. \(See step 11\.\)

   If you are using an Amazon EC2 instance as the VM for your cached volume gateway, see [ Detaching an Amazon EBS volume from a Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-detaching-volume.html) in the *Amazon EC2 User Guide for Linux Instances*\. For information about detaching disks from a KVM, VMware, or Hyper\-V VM, see the documentation for your hypervisor\.

1. Create a new storage gateway hypervisor VM instance, but don't activate it as a gateway\. For more information about creating a new storage gateway hypervisor VM, see [Set up a Volume Gateway](create-volume-gateway.md#set-up-gateway-volume)\. This new gateway will assume the identity of the old gateway\.
**Note**  
Do not add disks for cache or upload buffer to the new VM\. Your new VM will use the same cache disks and upload buffer disks that were used by the old VM\.

1. Your new storage gateway hypervisor VM instance should use the same network configuration as the old VM\. The default network configuration for the gateway is Dynamic Host Configuration Protocol \(DHCP\)\. With DHCP, your gateway is automatically assigned an IP address\.

   If you need to manually configure a static IP address for your new VM, see [Configuring Your Gateway Network](manage-on-premises-common.md#MaintenanceConfiguringStaticIP-common) for more details\. If your gateway must use a Socket Secure version 5 \(SOCKS5\) proxy to connect to the internet, see [Routing Your On\-Premises Gateway Through a Proxy](manage-on-premises-common.md#MaintenanceRoutingProxy-common) for more details\.

1. Start the new VM\.

1. Attach the disks that you detached from the old cached volume gateway VM in step 5, to the new cached volume gateway\. Attach them in the same order to the new gateway VM as they are on the old gateway VM\.

   All disks must make the transition unchanged\. Do not change volume sizes, as that will cause metadata to become inconsistent\.

1. Initiate the gateway migration process by connecting to the new VM with a URL that uses the following format\.

   ```
   http://your-VM-IP-address/migrate?gatewayId=your-gateway-ID
   ```

   You can re\-use the same IP address for the new gateway VM as you used for the old gateway VM\. Your URL should look similar to the example following\.

   ```
   http://198.51.100.123/migrate?gatewayId=sgw-12345678
   ```

   Use this URL from a browser, or from the command line using `curl`, to initiate the migration process\.

   When the gateway migration process is successfully initiated, you will see the following message:

   `Successfully imported Storage Gateway information. Please refer to Storage Gateway documentation to perform the next steps to complete the migration.`

1. Detach the old gateway's root disk, whose volume ID you noted in step 5\.

1. Start the gateway\.

   Use the following steps to start the new cached volume gateway:

   1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

   1. In the navigation pane, choose **Gateways** and then choose the new gateway you want to start\. The status of the gateway is **Shutdown**\.

   1. Choose **Details**, and then choose **Start gateway**\.

   For more information about starting a gateway, see [Starting and Stopping a Volume or Tape Gateway](MaintenanceShutDown-common.md#start-stop-classic)\.

1. Your volumes should now be available to your applications at the new gateway VM's IP address\.

1. Confirm that your volumes are available, and delete the old gateway VM\. For information about deleting a VM, see the documentation for your hypervisor\.

## Moving virtual tapes to a new tape gateway<a name="migrate-data-tape"></a>



**To move your virtual tape to a new tape gateway**

1. Use your backup application to back up all your data onto a virtual tape\. Wait for the backup to finish successfully\.

1. Use your backup application to eject your tape\. The tape will be stored in one of the Amazon S3 storage classes\. Ejected tapes are archived in S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive, and are read\-only\.

   Before proceeding, confirm that the ejected tapes have been archived:

   1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

   1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   1. In the **Status** column of the list, check the status of the tape\.

      The tape status also appears in the **Details** tab of each virtual tape\.

      For more information about determining tape status in an archive, see [Determining Tape Status in an Archive](managing-gateway-vtl.md#determine-tape-status-vts)\.

1. Using your backup application, verify that there are no active backup jobs going to the existing tape gateway before you stop it\. If there are any active backup jobs, wait for them to finish and eject your tapes \(see previous step\) before stopping the gateway\.

1. Use the following steps to stop the existing tape gateway:

   1. In the navigation pane, choose **Gateways**, and then choose the old tape gateway that you want to stop\. The status of the gateway is **Running**\.

   1. For **Actions**, choose **Stop gateway**\. Verify the ID of the gateway from the dialog box, and then choose **Stop gateway**\.

      While the old tape gateway is stopping, you might see a message that indicates the status of the gateway\. When the gateway shuts down, a message and a **Start gateway** button appear in the **Details** tab\.

   For more information about stopping a gateway, see [Starting and Stopping a Volume or Tape Gateway](MaintenanceShutDown-common.md#start-stop-classic)\.

1. Create a new tape gateway\. For detailed instructions, see [Creating a Gateway](create-gateway-vtl.md)\.

1. Use the following steps to create new tapes:

   1. In the navigation pane, choose the **Gateways** tab\.

   1. Choose **Create tape** to open the **Create tape** dialog box\.

   1. For **Gateway**, choose a gateway\. The tape is created for this gateway\.

   1. For **Number of tapes**, choose the number of tapes that you want to create\. For more information about tape limits, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

      You can also set up automatic tape creation at this point\. For more information, see [Creating Tapes Automatically](GettingStartedCreateTapes.md#CreateTapesAutomatically)\.

   1. For **Capacity**, enter the size of the virtual tape that you want to create\. Tapes must be larger than 100 GiB\. For information about capacity limits, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

   1. For **Barcode prefix**, enter the prefix that you want to prepend to the barcode of your virtual tapes\.
**Note**  
Virtual tapes are uniquely identified by a barcode\. You can add a prefix to the barcode\. The prefix is optional, but you can use it to help identify your virtual tapes\. The prefix must be uppercase letters \(A–Z\) and must be one to four characters long\.

   1. For **Pool**, choose **Glacier Pool** or **Deep Archive Pool**\. This pool represents the storage class in which your tape will be stored when it is ejected by your backup software\.

      Choose **Glacier Pool** if you want to archive the tape in S3 Glacier Flexible Retrieval\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives where you can retrieve a tape typically within 3\-5 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

      Choose **Deep Archive Pool** if you want to archive the tape in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation where data is accessed once or twice a year\. You can retrieve a tape archived in S3 Glacier Deep Archive typically within 12 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

      If you archive a tape in S3 Glacier Flexible Retrieval, you can move it to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.
**Note**  
Tapes created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects them\.

   1. \(Optional\) For **Tags**, enter a key and value to add tags to your tape\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your tapes\.

   1. Choose **Create tapes**\.

1. Use your backup application to start a backup job, and back up your data to the new tape\.

1. If your tape is archived and you need to restore data from it, retrieve it to the new tape gateway\. The tape will be in read\-only mode\. For more information about retrieving archived tapes, see [Retrieving Archived Tapes](managing-gateway-vtl.md#retrieving-archived-tapes-vtl)\.
**Note**  
Outbound data charges might apply\.

   1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

   1. Choose the virtual tape that you want to retrieve\. For **Actions**, choose **Retrieve Tape**\.
**Note**  
The status of the virtual tape that you want to retrieve must be `ARCHIVED`\.

   1. In the **Retrieve tape** dialog box, for **Barcode**, verify that the barcode identifies the virtual tape you want to retrieve\.

   1. For **Gateway**, choose the new tape gateway that you want to retrieve the archived tape to, and then choose **Retrieve tape**\.

   When you have confirmed that your new tape gateway is working correctly, you can delete the old tape gateway\.
**Important**  
Before you delete a gateway, be sure that there are no applications currently writing to that gateway's volumes\. If you delete a gateway while it is in use, data loss can occur\.

1. Use the following steps to delete the old tape gateway:
**Warning**  
When a gateway is deleted, there is no way to recover it\.

   1. In the navigation pane, choose **Gateways**, and then choose the gateway that you want to delete\.

   1. For **Actions**, choose **Delete gateway**\.

      In the confirmation dialog box that appears, make sure that the gateway ID listed specifies the old tape gateway that you want to delete, enter **delete** in the confirmation field, and then choose **Delete**\.

   1. Delete the VM\. For more information about deleting a VM, see the documentation for your hypervisor\.