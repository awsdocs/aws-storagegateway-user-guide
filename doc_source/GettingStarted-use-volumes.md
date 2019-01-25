# Using Your Volume<a name="GettingStarted-use-volumes"></a>

Following, you can find instructions about how to use your volume\. To use your volume, you first connect it to your client as an iSCSI target, then initialize and format it\.

**Topics**
+ [Connecting Your Volumes to Your Client](#GettingStartedAccessVolumes)
+ [Initializing and Formatting Your Volume](#format-volume)
+ [Testing Your Gateway](#GettingStartedTestGatewayMain)
+ [Where Do I Go from Here?](#GettingStartedWhatsNextStep3)

## Connecting Your Volumes to Your Client<a name="GettingStartedAccessVolumes"></a>

You use the iSCSI initiator in your client to connect to your volumes\. At the end of the following procedure, the volumes become available as local devices on your client\.

**Important**  
With AWS Storage Gateway, you can connect multiple hosts to the same volume if the hosts coordinate access by using Windows Server Failover Clustering \(WSFC\)\. You can't connect multiple hosts to the same volume without using WSFC, for example by sharing a nonclustered NTFS/ext4 file system\. 

**Topics**
+ [Connecting to a Microsoft Windows Client](#issci-windows)
+ [Connecting to a Red Hat Enterprise Linux Client](#issci-rhel)

### Connecting to a Microsoft Windows Client<a name="issci-windows"></a>

The following procedure shows a summary of the steps that you follow to connect to a Windows client\. For more information, see [Connecting iSCSI Initiators](initiator-connection-common.md)\.

**To connect to a Windows client**

1. Start iscsicpl\.exe\.

1. In the **iSCSI Initiator Properties** dialog box, choose the **Discovery** tab, and then choose **Discovery Portal**\.

1. In the **Discover Target Portal** dialog box, type the IP address of your iSCSI target for IP address or DNS name\. 

1. Connect the new target portal to the storage volume target on the gateway\.

1. Choose the target, and then choose **Connect**\.

1. In the **Targets** tab, make sure that the target status has the value **Connected**, indicating the target is connected, and then choose **OK**\. 

### Connecting to a Red Hat Enterprise Linux Client<a name="issci-rhel"></a>

The following procedure shows a summary of the steps that you follow to connect to a Red Hat Enterprise Linux \(RHEL\) client\. For more information, see [Connecting iSCSI Initiators](initiator-connection-common.md)\.

**To connect a Linux client to iSCSI targets**

1. Install the iscsi\-initiator\-utils RPM package\.

   You can use the following command to install the package\.

   ```
   sudo yum install iscsi-initiator-utils
   ```

1. Make sure that the iSCSI daemon is running\.

   For RHEL 5 or 6, use the following command\.

   ```
   sudo /etc/init.d/iscsi status
   ```

   For RHEL 7, use the following command\.

   ```
   sudo service iscsid status
   ```

1. Discover the volume or VTL device targets defined for a gateway\. Use the following discovery command\.

   ```
   sudo /sbin/iscsiadm --mode discovery --type sendtargets --portal [GATEWAY_IP]:3260
   ```

   The output of the discovery command should look like the following example output\.

   For volume gateways: `[GATEWAY_IP]:3260, 1 iqn.1997-05.com.amazon:myvolume `

   For tape gateways: `iqn.1997-05.com.amazon:[GATEWAY_IP]-tapedrive-01`

1. Connect to a target\. 

   Make sure to specify the correct *\[GATEWAY\_IP\]* and IQN in the connect command\.

   Use the following command\.

   ```
   sudo /sbin/iscsiadm --mode node --targetname iqn.1997-05.com.amazon:[ISCSI_TARGET_NAME] --portal [GATEWAY_IP]:3260,1 --login
   ```

1. Verify that the volume is attached to the client machine \(the initiator\)\. To do so, use the following command\.

   ```
   ls -l /dev/disk/by-path
   ```

   The output of the command should look like the following example output\.

   `lrwxrwxrwx. 1 root root 9 Apr 16 19:31 ip-[GATEWAY_IP]:3260-iscsi-iqn.1997-05.com.amazon:myvolume-lun-0 -> ../../sda`

   We highly recommend that after you set up your initiator you customize your iSCSI settings as discussed in [Customizing Your Linux iSCSI Settings](initiator-connection-common.md#CustomizeLinuxiSCSISettings)\.

## Initializing and Formatting Your Volume<a name="format-volume"></a>

After you use the iSCSI initiator in your client to connect to your volumes, you initialize and format your volume\.

**Topics**
+ [Initializing and Formatting Your Volume on Microsoft Windows](#format-windows)
+ [Initializing and Formatting Your Volume on Red Hat Enterprise Linux](#format-rhel)

### Initializing and Formatting Your Volume on Microsoft Windows<a name="format-windows"></a>

Use the following procedure to initialize and format your volume on Windows\.<a name="GettingStartedAccessVolumesFormatting"></a>

**To initialize and format your storage volume**

1. Start **diskmgmt\.msc** to open the **Disk Management** console\.

1. In the **Initialize Disk** dialog box, initialize the volume as a **MBR \(Master Boot Record\)** partition\. When selecting the partition style, you should take into account the type of volume you are connecting to—cached or stored—as shown in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/GettingStarted-use-volumes.html)

1. Create a simple volume:

   1. Bring the volume online to initialize it\. All the available volumes are displayed in the disk management console\. 

   1. Open the context \(right\-click\) menu for the disk, and then choose **New Simple Volume**\.
**Important**  
Be careful not to format the wrong disk\. Check to make sure that the disk you are formatting matches the size of the local disk you allocated to the gateway VM and that it has a status of **Unallocated**\. 

   1. Specify the maximum disk size\.

   1. Assign a drive letter or path to your volume, and format the volume by choosing **Perform a quick format**\.
**Important**  
We strongly recommend using **Perform a quick format** for cached volumes\. Doing so results in less initialization I/O, smaller initial snapshot size, and the fastest time to a usable volume\. It also avoids using cached volume space for the full format process\.
**Note**  
The time that it takes to format the volume depends on the size of the volume\. The process might take several minutes to complete\.

### Initializing and Formatting Your Volume on Red Hat Enterprise Linux<a name="format-rhel"></a>

Use the following procedure to initialize and format your volume on Red Hat Enterprise Linux \(RHEL\)\.

**To initialize and format your storage volume**

1. Change directory to the `/dev` folder\.

1. Run the `sudo cfdisk` command\.

1. Identify your new volume by using the following command\. To find new volumes, you can list the partition layout of your volumes\.

   `$ lsblk`

   An "unrecognized volumes label" error for the new unpartitioned volume appears\.

1. Initialize your new volume\. When selecting the partition style, you should take into account the size and type of volume you are connecting to—cached or stored—as shown in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/GettingStarted-use-volumes.html)

   For an MBR partition, use the following command: `sudo parted /dev/your volume mklabel msdos`

   For a GPT partition, use the following command: `sudo parted /dev/your volume mklabel gpt`

1. Create a partition by using the following command\.

   `sudo parted -a opt /dev/your volume mkpart primary file system 0% 100%`

1. Assign a drive letter to the partition and create a file system by using the following command\.

   `sudo mkfs drive letter datapartition /dev/your volume`

1. Mount the file system by using the following command\.

    `sudo mount -o defaults /dev/your volume /mnt/your directory` 

## Testing Your Gateway<a name="GettingStartedTestGatewayMain"></a>

You test your volume gateway setup by performing the following tasks:

1. Write data to the volume\.

1. Take a snapshot\.

1. Restore the snapshot to another volume\.

You verify the setup for a gateway by taking a snapshot backup of your volume and storing the snapshot in AWS\. You then restore the snapshot to a new volume\. Your gateway copies the data from the specified snapshot in AWS to the new volume\.

**Note**  
Restoring data from Amazon Elastic Block Store \(Amazon EBS\) volumes that are encrypted is not supported\.

**To create a snapshot of a storage volume on Microsoft Windows**

1. On your Windows computer, copy some data to your mapped storage volume\.

   The amount of data copied doesn't matter for this demonstration\. A small file is enough to demonstrate the restore process\.

1. In the navigation pane of the AWS Storage Gateway console, choose **Volumes**\.

1. Choose the storage volume that you created for the gateway\.

   This gateway should have only one storage volume\. Choose the volume displays its properties\.

1. For **Actions**, choose **Create Snapshot** to create a snapshot of the volume\.

   Depending on the amount of data on the disk and the upload bandwidth, it might take a few seconds to complete the snapshot\. Note the volume ID for the volume from which you create a snapshot\. You use the ID to find the snapshot\.

1. In the **Create Snapshot** dialog box, provide a description for your snapshot, and then choose **Create Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/create-snapshot.png)

   Your snapshot is stored as an Amazon EBS snapshot\. Take note of your snapshot ID\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/snapshotId.png)

   The number of snapshots created for your volume is displayed in the snapshot column\.

1. For **Snapshot**, choose the link for the volume you created the snapshot for to see your EBS snapshot on the Amazon EC2 console\.

## Where Do I Go from Here?<a name="GettingStartedWhatsNextStep3"></a>

In the preceding sections, you created and provisioned a gateway and then connected your host to the gateway's storage volume\. You added data to the gateway's iSCSI volume, took a snapshot of the volume, and restored it to a new volume, connected to the new volume, and verified that the data shows up on it\. 

After you finish the exercise, consider the following:
+ If you plan on continuing to use your gateway, read about sizing the upload buffer more appropriately for real\-world workloads\. For more information, see [Sizing Your Volume Gateway's Storage for Real\-World Workloads](#GettingStartedSizingForRealWorld)\.
+ If you don't plan on continuing to use your gateway, consider deleting the gateway to avoid incurring any charges\. For more information, see [Cleaning Up Resources You Don't Need](#cleanup)\. 

Other sections of this guide include information about how to do the following:
+ To learn more about storage volumes and how to manage them, see [Managing Your Gateway](managing-gateway-common.md)\.
+ To troubleshoot gateway problems, see [Troubleshooting Your Gateway](Troubleshooting-common.md)\.
+ To optimize your gateway, see [Optimizing Gateway Performance](Performance.md#Optimizing-common)\.
+ To learn about Storage Gateway metrics and how you can monitor how your gateway performs, see [Monitoring Your Gateway and Resources](Main_monitoring-gateways-common.md)\)\.
+ To learn more about configuring your gateway's iSCSI targets to store data, see [Connecting to Your Volumes to a Windows Client](initiator-connection-common.md#ConfiguringiSCSIClient)\.

To learn about sizing your volume gateway's storage for real\-world workloads and cleaning up resources you don't need, see the following sections\.

### Sizing Your Volume Gateway's Storage for Real\-World Workloads<a name="GettingStartedSizingForRealWorld"></a>

By this point, you have a simple, working gateway\. However, the assumptions used to create this gateway are not appropriate for real\-world workloads\. If you want to use this gateway for real\-world workloads, you need to do two things: 

1. Size your upload buffer appropriately\.

1. Set up monitoring for your upload buffer, if you haven't done so already\.

Following, you can find how to do both of these tasks\. If you activated a gateway for cached volumes, you also need to size your cache storage for real\-world workloads\.

**To size your upload buffer and cache storage for a gateway\-cached setup**
+ Use the formula shown in [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common) for sizing the upload buffer\. We strongly recommend that you allocate at least 150 GiB for the upload buffer\. If the upload buffer formula yields a value less than 150 GiB, use 150 GiB as your allocated upload buffer\.

  The upload buffer formula takes into account the difference between throughput from your application to your gateway and throughput from your gateway to AWS, multiplied by how long you expect to write data\. For example, assume that your applications write text data to your gateway at a rate of 40 MB per second for 12 hours a day and your network throughput is 12 MB per second\. Assuming a compression factor of 2:1 for the text data, the formula specifies that you need to allocate approximately 675 GiB of upload buffer space\.

**To size your upload buffer for a stored setup**
+ Use the formula discussed in [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\. We strongly recommend that you allocate at least 150 GiB for your upload buffer\. If the upload buffer formula yields a value less than 150 GiB, use 150 GiB as your allocated upload buffer\.

  The upload buffer formula takes into account the difference between throughput from your application to your gateway and throughput from your gateway to AWS, multiplied by how long you expect to write data\. For example, assume that your applications write text data to your gateway at a rate of 40 MB per second for 12 hours a day and your network throughput is 12 MB per second\. Assuming a compression factor of 2:1 for the text data, the formula specifies that you need to allocate approximately 675 GiB of upload buffer space\.

**To monitor your upload buffer**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose the **Gateway** tab, choose the **Details** tab, and then find the **Upload Buffer Used** field to view your gateway's current upload buffer\.

1. Set one or more alarms to notify you about upload buffer use\.

   We highly recommend that you create one or more upload buffer alarms in the Amazon CloudWatch console\. For example, you can set an alarm for a level of use you want to be warned about and an alarm for a level of use that, if exceeded, is cause for action\. The action might be adding more upload buffer space\. For more information, see [To set an upper threshold alarm for a gateway's upload buffer](Main_monitoring-gateways-common.md#GatewayAlarm1-common)\.

### Cleaning Up Resources You Don't Need<a name="cleanup"></a>

If you created your gateway as an example exercise or a test, consider cleaning up to avoid incurring unexpected or unnecessary charges\. 

**To clean up resources you don't need**

1. Delete any snapshots\. For instructions, see [Deleting a Snapshot](managing-volumes.md#DeletingASnapshot)\.

1. Unless you plan to continue using the gateway, delete it\. For more information, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

1. Delete the AWS Storage Gateway VM from your on\-premises host\. If you created your gateway on an Amazon EC2 instance, terminate the instance\. 