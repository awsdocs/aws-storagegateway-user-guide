# Managing Your Volume Gateway<a name="managing-volumes"></a>

Following, you can find information about how to manage your volume gateway resources\.

Cached volumes are volumes in Amazon Simple Storage Service \(Amazon S3\) that are exposed as iSCSI targets on which you can store your application data\. You can find information following about how to add and delete volumes for your cached setup\. You can also learn how to add and remove Amazon Elastic Block Store \(Amazon EBS\) volumes in Amazon EC2 gateways\.

**Topics**
+ [Adding a Volume](#ApplicationStorageVolumesCached-Adding)
+ [Expanding the Size of a Volume](#volume-size-increase)
+ [Cloning a Volume](#clone-volume)
+ [Viewing Volume Usage](#volume-usage)
+ [Deleting a Volume](#ApplicationStorageVolumesCached-Removing)
+ [Moving Your Volumes to a Different Gateway](#attach-detach-volume)
+ [Reducing the Amount of Billed Storage on a Volume](#reduce-bill-volume)
+ [Creating a One\-Time Snapshot](#CreatingSnapshot)
+ [Editing a Snapshot Schedule](#SchedulingSnapshot)
+ [Deleting a Snapshot](#DeletingASnapshot)
+ [Understanding Volume Statuses and Transitions](#StorageVolumeStatuses)

**Important**  
If a cached volume keeps your primary data in Amazon S3, you should avoid processes that read or write all data on the entire volume\. For example, we don't recommend using virus\-scanning software that scans the entire cached volume\. Such a scan, whether done on demand or scheduled, causes all data stored in Amazon S3 to be downloaded locally for scanning, which results in high bandwidth usage\. Instead of doing a full disk scan, you can use real\-time virus scanning—that is, scanning data as it is read from or written to the cached volume\.

Resizing a volume is not supported\. To change the size of a volume, create a snapshot of the volume, and then create a new cached volume from the snapshot\. The new volume can be bigger than the volume from which the snapshot was created\. For steps describing how to remove a volume, see [To remove a volume](#CachedRemovingAStorageVolume)\. For steps describing how to add a volume and preserve existing data, see [Deleting a Volume](#ApplicationStorageVolumesCached-Removing)\.

All cached volume data and snapshot data is stored in Amazon S3 and is encrypted at rest using server\-side encryption \(SSE\)\. However, you cannot access this data by using the Amazon S3 API or other tools such as the Amazon S3 Management Console\.

## Adding a Volume<a name="ApplicationStorageVolumesCached-Adding"></a>

As your application needs grow, you might need to add more volumes to your gateway\. As you add more volumes, you must consider the size of the cache storage and upload buffer you allocated to the gateway\. The gateway must have sufficient buffer and cache space for new volumes\. For more information, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\. 

You can add volumes using the AWS Storage Gateway console or AWS Storage Gateway API\. For information on using the AWS Storage Gateway API to add volumes, see [CreateCachediSCSIVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateCachediSCSIVolume.html)\. For instructions on how to add a volume using the AWS Storage Gateway console, see [Creating a Volume](GettingStartedCreateVolumes.md)\.

## Expanding the Size of a Volume<a name="volume-size-increase"></a>

As your application needs grow, you might want to expand your volume instead of adding more volumes to your gateway\. In this case, you can do one of the following:
+ Create a snapshot of the volume you want to expand and then use the snapshot to create a new volume of a larger size\. For information about how to create a snapshot, see [Creating a One\-Time Snapshot](#CreatingSnapshot)\. For information about how to use a snapshot to create a new volume, see [Creating a Volume](GettingStartedCreateVolumes.md)\. 
+ Use the cached volume you want to expand to clone a new volume of a larger size\. For information about how to clone a volume, see [Cloning a Volume](#clone-volume)\. For information about how to create a volume, see [Creating a Volume](GettingStartedCreateVolumes.md)\. 

## Cloning a Volume<a name="clone-volume"></a>

You can create a new volume from any existing cached volume in the same AWS Region\. The new volume is created from the most recent recovery point of the selected volume\. A *volume recovery point* is a point in time at which all data of the volume is consistent\. To clone a volume, you choose the **Clone from last recovery point** option in the **Create volume** dialog box, then select the volume to use as the source\. The following screenshot shows the **Create volume** dialog box\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/clone-volume.png)

Cloning from an existing volume is faster and more cost\-effective than creating an Amazon EBS snapshot\. Cloning does a byte\-to\-byte copy of your data from the source volume to the new volume, using the most recent recovery point from the source volume\. Storage Gateway automatically creates recovery points for your cached volumes\. To see when the last recovery point was created, check the `TimeSinceLastRecoveryPoint` metric in Amazon CloudWatch\. 

The cloned volume is independent of the source volume\. That is, changes made to either volume after cloning have no effect on the other\. For example, if you delete the source volume, it has no effect on the cloned volume\. You can clone a source volume while initiators are connected and it is in active use\. Doing so doesn't affect the performance of the source volume\. For information about how to clone a volume, see [Creating a Volume](GettingStartedCreateVolumes.md)\.

 You can also use the cloning process in recovery scenarios\. For more information, see [Your Cached Gateway is Unreachable And You Want to Recover Your Data](troubleshoot-volume-issues.md#RecoverySnapshotTroubleshooting)\.

### Cloning from a Volume Recovery Point<a name="cloning"></a>

The following procedure shows you how to clone a volume from a volume recovery point and use that volume\.

**To clone and use a volume from an unreachable gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the AWS Storage Gateway console, choose **Create volume**\.

1. In the **Create volume** dialog box, choose a gateway for **Gateway**\. 

1. For **Capacity**, type the capacity for your volume\. The capacity must be at least the same size as the source volume\.

1. Choose **Clone from last recovery point** and select a volume ID for **Source volume**\. The source volume can be any cached volume in the selected AWS Region\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/clone-volume.png)

1. Type a name for **iSCSI target name**\.

   The target name can contain lowercase letters, numbers, periods \(\.\), and hyphens \(\-\)\. This target name appears as the **iSCSI target node** name in the **Targets** tab of the **iSCSI Microsoft initiator** UI after discovery\. For example, the name `target1` appears as `iqn.1007-05.com.amazon:target1`\. Ensure that the target name is globally unique within your storage area network \(SAN\)\. 

1. Verify that the **Network interface** setting is the IP address of your gateway, or choose an IP address for **Network interface**\.

   If you have defined your gateway to use multiple network adapters, choose the IP address that your storage applications use to access the volume\. Each network adapter defined for a gateway represents one IP address that you can choose\.

   If the gateway VM is configured for more than one network adapter, the **Create volume** dialog box displays a list for **Network interface**\. In this list, one IP address appears for each adapter configured for the gateway VM\. If the gateway VM is configured for only one network adapter, no list appears because there's only one IP address\.

1. Choose **Create volume**\. The **Configure CHAP Authentication** dialog box appears\. You can configure CHAP later\. For information, see [Configuring CHAP Authentication for Your iSCSI Targets](initiator-connection-common.md#ConfiguringiSCSIClientInitiatorCHAP)\.

The next step is to connect your volume to your client\. For more information, see [Connecting Your Volumes to Your Client](GettingStarted-use-volumes.md#GettingStartedAccessVolumes)\.

### Creating a Recovery Snapshot<a name="snapshot"></a>

The following procedure shows you how to create a snapshot from a volume recovery point and using that snapshot\. You can take snapshots on a one\-time, ad hoc basis or set up a snapshot schedule for the volume\. 

**To create and use a recovery snapshot of a volume from an unreachable gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. Choose the unreachable gateway, and then choose the **Details** tab\.

   A recovery snapshot message is displayed in the tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/recorvery-snapshot-message.png)

1. Choose **Create recovery snapshot** to open the **Create recovery snapshot** dialog box\.

1. From the list of volumes displayed, choose the volume you want to recover, and then choose **Create snapshots**\.

   AWS Storage Gateway initiates the snapshot process\.

1. Find and restore the snapshot\.

## Viewing Volume Usage<a name="volume-usage"></a>

When you write data to a volume, you can view the amount of data stored on the volume in the AWS Storage Gateway Management Console\. The **Details** tab for each volume shows the volume usage information\.

**To view amount of data written to a volume**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Volumes** and then choose the volume you are interested in\.

1. Choose the **Details** tab\.

   The following fields provide information about the volume:
   + **Size:** The total capacity of the selected volume\.
   + **Used:** The size of data stored on the volume\. 
**Note**  
These values are not available for volumes created before May 13, 2015, until you store data on the volume\.

## Deleting a Volume<a name="ApplicationStorageVolumesCached-Removing"></a>

You might need to delete a volume as your application needs change—for example, if you migrate your application to use a larger storage volume\. Before you delete a volume, make sure that there are no applications currently writing to the volume\. Also, make sure that there are no snapshots in progress for the volume\. If a snapshot schedule is defined for the volume, you can check it on the **Snapshot Schedules** tab of the AWS Storage Gateway console\. For more information, see [Editing a Snapshot Schedule](#SchedulingSnapshot)\. 

You can delete volumes using the AWS Storage Gateway console or the AWS Storage Gateway API\. For information on using the AWS Storage Gateway API to remove volumes, see [Delete Volume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteVolume.html)\. The following procedure demonstrates using the console\. 

Before you delete a volume, back up your data or take a snapshot of your critical data\. For stored volumes, your local disks aren't erased\. After you delete a volume, you can't get it back\.<a name="CachedRemovingAStorageVolume"></a>

**To remove a volume**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the **Volumes** tab, choose the volume and choose the confirmation box\. Make sure that the volume listed is the volume you intend to delete\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/delete-volume.png)

1. Choose **Delete** to delete the volume\.

## Moving Your Volumes to a Different Gateway<a name="attach-detach-volume"></a>

As your data and performance needs grow, you might want to move your volumes to a different volume gateway\. To do so, you can detach and attach a volume by using the Storage Gateway console or API\. 

By detaching and attaching a volume, you can do the following:
+ Move your volumes to better host platforms or newer Amazon EC2 instances\.
+ Refresh the underlying hardware for your server\.
+ Move your volumes between hypervisor types\.

When you detach a volume, your gateway uploads and stores the volume data and metadata to the AWS Storage Gateway service in AWS\. You can easily attach a detached volume to a gateway on any supported host platform later\. 

**Note**  
A detached volume is billed at the standard volume storage rate until you delete it\. For information about how to reduce your bill, see [Reducing the Amount of Billed Storage on a Volume](#reduce-bill-volume)\.

**Note**  
There are some limitations for attaching and detaching volumes:  
Detaching a volume can take a long time\. When you detach a volume, the gateway uploads all the data on the volume to AWS before the volume is detached\. The time it takes for the upload to complete depends on how much data needs to be uploaded and your network connectivity into AWS\.
If you detach a cached volume, you can't reattach it as a stored volume\.
If you detach a stored volume, you can't reattach it as a cached volume\.
 A detached volume can't be used until it is attached to a gateway\.
When you attach a stored volume, it needs to fully restore before you can attach it to a gateway\.
When you start attaching or detaching a volume, you need to wait till the operation completed before you use the volume\. 
Currently, forcibly deleting a volume is only supported in the API\.
If you delete a gateway while your volume is detaching from that gateway, it results in data loss\. Wait until the volume detach operation is complete before you delete the gateway\.
If a stored gateway is in restoring state, you can't detach a volume from it\.

The following steps show you how to detach and attach a volume using the Storage Gateway console\. For more information about doing this using the API, see [DetachVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DetachVolume.html) or [AttachVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AttachVolume.html) in the *AWS Storage Gateway API Reference\.* 

**To detach a volume from a gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Volumes**\.

1. From the list of volumes, choose the volume that you want to detach\. You can choose multiple volumes to detach multiple volumes at a time\.

1. For **Actions**, choose **Detach volume**\. The volumes that you chose are listed in the **Detach Volume** dialog box that appears\. Make sure that only the volumes you want to detach are listed\.

1. Choose **Detach volume**\. If a volume that you detach has a lot of data on it, it transitions from **Attached** to **Detaching** status until it finishes uploading all the data\. Then the status changes to **Detached**\. For small amounts of data, you might not see the **Detaching** status\. If the volume doesn't have data on it, the status changes from **Attached** to **Detached**\.

You can now attach the volume to a different gateway\.

**To attach a volume to a gateway**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Volumes**\. The status of each volume that is detached shows as **Detached**\.

1. From the list of detached volumes, choose the volume that you want to attach\. You can attach only one volume at a time\.

1. For **Actions**, choose **Attach volume**\. 

1. In the **Attach Volume** dialog box, choose the gateway that you want to attach the volume to, and then enter the iSCSI target that you want to connect the volume to\.

   If you are attaching a stored volume, enter its disk identifier for **Disk ID**\.

1. Choose **Attach volume**\. If a volume that you attach has a lot of data on it, it transitions from **Detached** to **Attached** if the `AttachVolume` operation succeeds\.

1. In the Configure CHAP authentication wizard that appears, enter the **Initiator name**, **Initiator secret**, and **Target secret**, and then choose **Save**\. For more information about working with Challenge\-Handshake Authentication Protocol \(CHAP\) authentication, see [Configuring CHAP Authentication for Your iSCSI Targets](initiator-connection-common.md#ConfiguringiSCSIClientInitiatorCHAP)\.

## Reducing the Amount of Billed Storage on a Volume<a name="reduce-bill-volume"></a>

Deleting files from your file system doesn't necessarily delete data from the underlying block device or reduce the amount of data stored on your volume\. If you want to reduce the amount of billed storage on your volume, we recommend overwriting your files with zeros to compress the storage to a negligible amount of actual storage\. AWS Storage Gateway charges for volume usage based on compressed storage\. 

**Note**  
If you use a delete tool that overwrites the data on your volume with random data, your usage will not be reduced\. This is because the random data is not compressible\.

## Creating a One\-Time Snapshot<a name="CreatingSnapshot"></a>

In addition to scheduled snapshots, for volume gateways you can take one\-time, ad hoc snapshots\. By doing this, you can back up your storage volume immediately without waiting for the next scheduled snapshot\.

**To take a one\-time snapshot of your storage volume**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Volumes**, and then choose the volume you want to create the snapshot from\.

1. For **Actions**, choose **Create snapshot**\.

1. In the **Create snapshot** dialog box, type the snapshot description, and then choose **Create snapshot**\.

   You can verify that the snapshot was created using the console\.

Your snapshot is listed in the **Snapshots** in the same row as the volume\.

## Editing a Snapshot Schedule<a name="SchedulingSnapshot"></a>

For stored volumes, AWS Storage Gateway creates a default snapshot schedule of once a day\. 

**Note**  
You can't remove the default snapshot schedule\. Stored volumes require at least one snapshot schedule\. However, you can change a snapshot schedule by specifying either the time the snapshot occurs each day or the frequency \(every 1, 2, 4, 8, 12, or 24 hours\), or both\.

For cached volumes, AWS Storage Gateway doesn't create a default snapshot schedule\. No default schedule is created because your data is stored in Amazon S3, so you don't need snapshots or a snapshot schedule for disaster recovery purposes\. However, you can set up a snapshot schedule at any time if you need to\. Creating snapshot for your cached volume provides an additional way to recover your data if necessary\.

By using the following steps, you can edit the snapshot schedule for a volume\.

**To edit the snapshot schedule for a volume**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Volumes**, and then choose the volume the snapshot was created from\.

1. For **Actions**, choose **Edit snapshot schedule**\.

1. In the **Edit snapshot schedule** dialog box, modify the schedule, and then choose **Save**\.

## Deleting a Snapshot<a name="DeletingASnapshot"></a>

You can delete a snapshot of your storage volume\. For example, you might want to do this if you have taken many snapshots of a storage volume over time and you don't need the older snapshots\. Because snapshots are incremental backups, if you delete a snapshot, only the data that is not needed in other snapshots is deleted\. 

**Topics**
+ [Deleting Snapshots by Using the AWS SDK for Java](#DeletingSnapshotsUsingJava)
+ [Deleting Snapshots by Using the AWS SDK for \.NET](#DeletingSnapshotsUsingDotNet)
+ [Deleting Snapshots by Using the AWS Tools for Windows PowerShell](#DeletingSnapshotsUsingPowerShell)

On the Amazon EBS console, you can delete snapshots one at a time\. For information about how to delete snapshots using the Amazon EBS console, see [Deleting an Amazon EBS Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-snapshot.html) in the *Amazon EC2 User Guide\.* 

 To delete multiple snapshots at a time, you can use one of the AWS SDKs that supports AWS Storage Gateway operations\. For examples, see [Deleting Snapshots by Using the AWS SDK for Java](#DeletingSnapshotsUsingJava), [Deleting Snapshots by Using the AWS SDK for \.NET](#DeletingSnapshotsUsingDotNet), and [Deleting Snapshots by Using the AWS Tools for Windows PowerShell](#DeletingSnapshotsUsingPowerShell)\.

### Deleting Snapshots by Using the AWS SDK for Java<a name="DeletingSnapshotsUsingJava"></a>

To delete many snapshots associated with a volume, you can use a programmatic approach\. The example following demonstrates how to delete snapshots using the AWS SDK for Java\. To use the example code, you should be familiar with running a Java console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-setup.html) in the *AWS SDK for Java Developer Guide*\. If you need to just delete a few snapshots, use the console as described in [Deleting a Snapshot](#DeletingASnapshot)\.

**Example : Deleting Snapshots by Using the AWS SDK for Java**  
The following Java code example lists the snapshots for each volume of a gateway and whether the snapshot start time is before or after a specified date\. It uses the AWS SDK for Java API for AWS Storage Gateway and Amazon EC2\. The Amazon EC2 API includes operations for working with snapshots\.   
Update the code to provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the number of days back you want to save snapshots\. Snapshots taken before this cutoff are deleted\. You also need to specify the Boolean value `viewOnly`, which indicates whether you want to view the snapshots to be deleted or to actually perform the snapshot deletions\. Run the code first with just the view option \(that is, with `viewOnly` set to `true`\) to see what the code deletes\. For a list of AWS service endpoints you can use with AWS Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.   

```
import java.io.IOException;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Collection;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.List;

import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.ec2.AmazonEC2Client;
import com.amazonaws.services.ec2.model.DeleteSnapshotRequest;
import com.amazonaws.services.ec2.model.DescribeSnapshotsRequest;
import com.amazonaws.services.ec2.model.DescribeSnapshotsResult;
import com.amazonaws.services.ec2.model.Filter;
import com.amazonaws.services.ec2.model.Snapshot;
import com.amazonaws.services.storagegateway.AWSStorageGatewayClient;
import com.amazonaws.services.storagegateway.model.ListVolumesRequest;
import com.amazonaws.services.storagegateway.model.ListVolumesResult;
import com.amazonaws.services.storagegateway.model.VolumeInfo;

public class ListDeleteVolumeSnapshotsExample {

    public static AWSStorageGatewayClient sgClient;
    public static AmazonEC2Client ec2Client;    
    static String serviceURLSG = "https://storagegateway.us-east-1.amazonaws.com";
    static String serviceURLEC2 = "https://ec2.us-east-1.amazonaws.com"; 
    
    // The gatewayARN
    public static String gatewayARN = "*** provide gateway ARN ***";
    
    // The number of days back you want to save snapshots. Snapshots before this cutoff are deleted
    // if viewOnly = false.
    public static int daysBack = 10;
    
    // true = show what will be deleted; false = actually delete snapshots that meet the daysBack criteria
    public static boolean viewOnly = true;

    public static void main(String[] args) throws IOException {

        // Create a storage gateway and amazon ec2 client
        sgClient = new AWSStorageGatewayClient(new PropertiesCredentials(
                ListDeleteVolumeSnapshotsExample.class.getResourceAsStream("AwsCredentials.properties")));    
        sgClient.setEndpoint(serviceURLSG);
        
        ec2Client = new AmazonEC2Client(new PropertiesCredentials(
                ListDeleteVolumeSnapshotsExample.class.getResourceAsStream("AwsCredentials.properties")));
        ec2Client.setEndpoint(serviceURLEC2);        
        
        List<VolumeInfo> volumes = ListVolumesForGateway();
        DeleteSnapshotsForVolumes(volumes, daysBack);        
        
    }
    public static List<VolumeInfo> ListVolumesForGateway()
    {
        List<VolumeInfo> volumes = new ArrayList<VolumeInfo>();

        String marker = null;
        do {
            ListVolumesRequest request = new ListVolumesRequest().withGatewayARN(gatewayARN);
            ListVolumesResult result = sgClient.listVolumes(request);
            marker = result.getMarker();
            
            for (VolumeInfo vi : result.getVolumeInfos())
            {
                volumes.add(vi);
                System.out.println(OutputVolumeInfo(vi));
            }           
        } while (marker != null);

        return volumes;
    }
    private static void DeleteSnapshotsForVolumes(List<VolumeInfo> volumes,
            int daysBack2) {
        
        // Find snapshots and delete for each volume 
        for (VolumeInfo vi : volumes) {
            
            String volumeARN = vi.getVolumeARN();
            String volumeId = volumeARN.substring(volumeARN.lastIndexOf("/")+1).toLowerCase();
            Collection<Filter> filters = new ArrayList<Filter>();
            Filter filter = new Filter().withName("volume-id").withValues(volumeId);
            filters.add(filter);
            
            DescribeSnapshotsRequest describeSnapshotsRequest =
                new DescribeSnapshotsRequest().withFilters(filters);
            DescribeSnapshotsResult describeSnapshotsResult =
                ec2Client.describeSnapshots(describeSnapshotsRequest);
            
            List<Snapshot> snapshots = describeSnapshotsResult.getSnapshots();
            System.out.println("volume-id = " + volumeId);
            for (Snapshot s : snapshots){
                StringBuilder sb = new StringBuilder();
                boolean meetsCriteria = !CompareDates(daysBack, s.getStartTime());
                sb.append(s.getSnapshotId() + ", " + s.getStartTime().toString());                
                sb.append(", meets criteria for delete? " + meetsCriteria);
                sb.append(", deleted? ");
                if (!viewOnly & meetsCriteria) {
                    sb.append("yes");
                    DeleteSnapshotRequest deleteSnapshotRequest = 
                        new DeleteSnapshotRequest().withSnapshotId(s.getSnapshotId());
                    ec2Client.deleteSnapshot(deleteSnapshotRequest);
                }
                else {
                    sb.append("no");
                }
                System.out.println(sb.toString());                    
            }        
        }
    }

    private static String OutputVolumeInfo(VolumeInfo vi) {
        
        String volumeInfo = String.format(
                 "Volume Info:\n" + 
                 "  ARN: %s\n" +
                 "  Type: %s\n",
                 vi.getVolumeARN(),
                 vi.getVolumeType());
        return volumeInfo; 
     }
    
    // Returns the date in two formats as a list
    public static boolean CompareDates(int daysBack, Date snapshotDate) {
        Date today = new Date();
        Calendar cal = new GregorianCalendar();
        cal.setTime(today);
        cal.add(Calendar.DAY_OF_MONTH, -daysBack);
        Date cutoffDate = cal.getTime();        
        return (snapshotDate.compareTo(cutoffDate) > 0) ? true : false;
    }

}
```

### Deleting Snapshots by Using the AWS SDK for \.NET<a name="DeletingSnapshotsUsingDotNet"></a>

To delete many snapshots associated with a volume, you can use a programmatic approach\. The following example demonstrates how to delete snapshots using the AWS SDK for \.NET version 2 and 3\. To use the example code, you should be familiar with running a \.NET console application\. For more information, see [Getting Started](https://docs.aws.amazon.com/AWSSdkDocsNET/latest/DeveloperGuide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\. If you need to just delete a few snapshots, use the console as described in [Deleting a Snapshot](#DeletingASnapshot)\.

**Example : Deleting Snapshots by Using the AWS SDK for \.NET**  
In the following C\# code example, an AWS Identity and Access Management \(IAM\) user can list the snapshots for each volume of a gateway\. The user can then determine whether the snapshot start time is before or after a specified date \(retention period\) and delete snapshots that have passed the retention period\. The example uses the AWS SDK for \.NET API for AWS Storage Gateway and Amazon EC2\. The Amazon EC2 API includes operations for working with snapshots\.   
The following code example uses the AWS SDK for \.NET version 2 and 3\. You can migrate older versions of \.NET to the newer version\. For more information, see [Migrating Your Code to the Latest Version of the AWS SDK for \.NET](https://docs.aws.amazon.com/AWSSdkDocsNET/latest/DeveloperGuide/net-dg-migration-guide-v2.html#net-dg-migrate-v2-new)\.  
Update the code to provide the service endpoint, your gateway Amazon Resource Name \(ARN\), and the number of days back you want to save snapshots\. Snapshots taken before this cutoff are deleted\. You also need to specify the Boolean value `viewOnly`, which indicates whether you want to view the snapshots to be deleted or to actually perform the snapshot deletions\. Run the code first with just the view option \(that is, with `viewOnly` set to `true`\) to see what the code deletes\. For a list of AWS service endpoints you can use with AWS Storage Gateway, see [AWS Storage Gateway Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.   
 First, you create an IAM user and attach the minimum IAM policy to the IAM user\. Then you schedule automated snapshots for your gateway\.   
The following code creates the minimum policy that allows an IAM user to delete snapshots\. In this example, the policy is named **sgw\-delete\-snapshot**\.  

```
{
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "StmtEC2Snapshots",
              "Effect": "Allow",
              "Action": [
                  "ec2:DeleteSnapshot",
                  "ec2:DescribeSnapshots"
              ],
              "Resource": [
                  "*"
              ]
          },
          {
              "Sid": "StmtSgwListVolumes",
              "Effect": "Allow",
              "Action": [
                  "storagegateway:ListVolumes"
              ],
              "Resource": [
                  "*"
              ]
          }
      ]
  }
```
The following C\# code finds all snapshots in the specified gateway that match the volumes and the specified cut\-off period and then deletes them\.   

```
using System;
using System.Collections.Generic;
using System.Text;
using Amazon.EC2;
using Amazon.EC2.Model;
using Amazon.StorageGateway.Model;
using Amazon.StorageGateway;

namespace DeleteStorageGatewaySnapshotNS
{
    class Program
    {
        /*
         * Replace the variables below to match your environment.
         */
         
         /* IAM AccessKey */
        static String AwsAccessKey = "AKIA................";
        
        /* IAM SecretKey */
        static String AwsSecretKey = "***********************";
        
        /* AWS Account number, 12 digits, no hyphen */
        static String OwnerID = "123456789012";
        
        /* Your Gateway ARN. Use a Storage Gateway ID, sgw-XXXXXXXX* */ 
        static String GatewayARN = "arn:aws:storagegateway:ap-southeast-2:123456789012:gateway/sgw-XXXXXXXX";
        
        /* Snapshot status: "completed", "pending", "error" */                                                                                                      
        static String SnapshotStatus = "completed";
        
        /* AWS Region where your gateway is activated */ 
        static String AwsRegion = "ap-southeast-2";
        
        /* Minimum age of snapshots before they are deleted (retention policy) */
        static int daysBack = 30; 

        /*
         * Do not modify the four lines below.
         */
        static AmazonEC2Config ec2Config;
        static AmazonEC2Client ec2Client;
        static AmazonStorageGatewayClient sgClient;
        static AmazonStorageGatewayConfig sgConfig;

        static void Main(string[] args)
        {
            // Create an EC2 client.
            ec2Config = new AmazonEC2Config();
            ec2Config.ServiceURL = "https://ec2." + AwsRegion + ".amazonaws.com";
            ec2Client = new AmazonEC2Client(AwsAccessKey, AwsSecretKey, ec2Config);

            // Create a Storage Gateway client.
            sgConfig = new AmazonStorageGatewayConfig();
            sgConfig.ServiceURL = "https://storagegateway." + AwsRegion + ".amazonaws.com";
            sgClient = new AmazonStorageGatewayClient(AwsAccessKey, AwsSecretKey, sgConfig);

            List<VolumeInfo> StorageGatewayVolumes = ListVolumesForGateway();
            List<Snapshot> StorageGatewaySnapshots = ListSnapshotsForVolumes(StorageGatewayVolumes, 
                                                     daysBack);
            DeleteSnapshots(StorageGatewaySnapshots);
        }

        /*
         * List all volumes for your gateway
         * returns: A list of VolumeInfos, or null.
         */
        private static List<VolumeInfo> ListVolumesForGateway()
        {
            ListVolumesResponse response = new ListVolumesResponse();
            try
            {
                ListVolumesRequest request = new ListVolumesRequest();
                request.GatewayARN = GatewayARN;
                response = sgClient.ListVolumes(request);

                foreach (VolumeInfo vi in response.VolumeInfos)
                {
                    Console.WriteLine(OutputVolumeInfo(vi));
                }
            }
            catch (AmazonStorageGatewayException ex)
            {
                Console.WriteLine(ex.Message);
            }
            return response.VolumeInfos;
        }

        /*
         * Gets the list of snapshots that match the requested volumes
         * and cutoff period.
         */
        private static List<Snapshot> ListSnapshotsForVolumes(List<VolumeInfo> volumes, int snapshotAge)
        {
            List<Snapshot> SelectedSnapshots = new List<Snapshot>();
            try
            {
                foreach (VolumeInfo vi in volumes)
                {
                    String volumeARN = vi.VolumeARN;
                    String volumeID = volumeARN.Substring(volumeARN.LastIndexOf("/") + 1).ToLower();

                    DescribeSnapshotsRequest describeSnapshotsRequest = new DescribeSnapshotsRequest();

                    Filter ownerFilter = new Filter();
                    List<String> ownerValues = new List<String>();
                    ownerValues.Add(OwnerID);
                    ownerFilter.Name = "owner-id";
                    ownerFilter.Values = ownerValues;
                    describeSnapshotsRequest.Filters.Add(ownerFilter);

                    Filter statusFilter = new Filter();
                    List<String> statusValues = new List<String>();
                    statusValues.Add(SnapshotStatus);
                    statusFilter.Name = "status";
                    statusFilter.Values = statusValues;
                    describeSnapshotsRequest.Filters.Add(statusFilter);

                    Filter volumeFilter = new Filter();
                    List<String> volumeValues = new List<String>();
                    volumeValues.Add(volumeID);
                    volumeFilter.Name = "volume-id";
                    volumeFilter.Values = volumeValues;
                    describeSnapshotsRequest.Filters.Add(volumeFilter);

                    DescribeSnapshotsResponse describeSnapshotsResponse = 
                      ec2Client.DescribeSnapshots(describeSnapshotsRequest);

                    List<Snapshot> snapshots = describeSnapshotsResponse.Snapshots;
                    Console.WriteLine("volume-id = " + volumeID);
                    foreach (Snapshot s in snapshots)
                    {
                        if (IsSnapshotPastRetentionPeriod(snapshotAge, s.StartTime))
                        {
                            Console.WriteLine(s.SnapshotId + ", " + s.VolumeId + ", 
                               " + s.StartTime + ", " + s.Description);
                            SelectedSnapshots.Add(s);
                        }
                    }
                }
            }
            catch (AmazonEC2Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            return SelectedSnapshots;
        }

        /*
         * Deletes a list of snapshots.
         */
        private static void DeleteSnapshots(List<Snapshot> snapshots)
        {
            try
            {
                foreach (Snapshot s in snapshots)
                {

                    DeleteSnapshotRequest deleteSnapshotRequest = new DeleteSnapshotRequest(s.SnapshotId);
                    DeleteSnapshotResponse response = ec2Client.DeleteSnapshot(deleteSnapshotRequest);
                    Console.WriteLine("Volume: " +
                              s.VolumeId +
                              " => Snapshot: " +
                              s.SnapshotId +
                              " Response: "
                              + response.HttpStatusCode.ToString());
                }
            }
            catch (AmazonEC2Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        /*
         * Checks if the snapshot creation date is past the retention period.
         */
        private static Boolean IsSnapshotPastRetentionPeriod(int daysBack, DateTime snapshotDate)
        {
            DateTime cutoffDate = DateTime.Now.Add(new TimeSpan(-daysBack, 0, 0, 0));
            return (DateTime.Compare(snapshotDate, cutoffDate) < 0) ? true : false;
        }

        /*
         * Displays information related to a volume.
         */
        private static String OutputVolumeInfo(VolumeInfo vi)
        {
            String volumeInfo = String.Format(
                "Volume Info:\n" +
                "  ARN: {0}\n" +
                "  Type: {1}\n",
                vi.VolumeARN,
                vi.VolumeType);
            return volumeInfo;
        }
    }
}
```

### Deleting Snapshots by Using the AWS Tools for Windows PowerShell<a name="DeletingSnapshotsUsingPowerShell"></a>

To delete many snapshots associated with a volume, you can use a programmatic approach\. The example following demonstrates how to delete snapshots using the AWS Tools for Windows PowerShell\. To use the example script, you should be familiar with running a PowerShell script\. For more information, see [Getting Started](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html) in the *AWS Tools for Windows PowerShell*\. If you need to delete just a few snapshots, use the console as described in [Deleting a Snapshot](#DeletingASnapshot)\.

**Example : Deleting Snapshots by Using the AWS Tools for Windows PowerShell**  
The following PowerShell script example lists the snapshots for each volume of a gateway and whether the snapshot start time is before or after a specified date\. It uses the AWS Tools for Windows PowerShell cmdlets for AWS Storage Gateway and Amazon EC2\. The Amazon EC2 API includes operations for working with snapshots\.  
You need to update the script and provide your gateway Amazon Resource Name \(ARN\) and the number of days back you want to save snapshots\. Snapshots taken before this cutoff are deleted\. You also need to specify the Boolean value `viewOnly`, which indicates whether you want to view the snapshots to be deleted or to actually perform the snapshot deletions\. Run the code first with just the view option \(that is, with `viewOnly` set to `true`\) to see what the code deletes\.   

```
<#
.DESCRIPTION
    Delete snapshots of a specified volume that match given criteria.
    
.NOTES
    PREREQUISITES:
    1) AWS Tools for PowerShell from https://aws.amazon.com/powershell/
    2) Credentials and AWS Region stored in session using Initialize-AWSDefault.
    For more info see, https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html 

.EXAMPLE
    powershell.exe .\SG_DeleteSnapshots.ps1  
#>

# Criteria to use to filter the results returned.
$daysBack = 18
$gatewayARN = "*** provide gateway ARN ***"
$viewOnly = $true;

#ListVolumes
$volumesResult = Get-SGVolume -GatewayARN $gatewayARN
$volumes = $volumesResult.VolumeInfos
Write-Output("`nVolume List")
foreach ($volumes in $volumesResult)
  { Write-Output("`nVolume Info:")
    Write-Output("ARN:  " + $volumes.VolumeARN)
    write-Output("Type: " + $volumes.VolumeType)
  }

Write-Output("`nWhich snapshots meet the criteria?")
foreach ($volume in $volumesResult)
  { 
    $volumeARN = $volume.VolumeARN
    
    $volumeId = ($volumeARN-split"/")[3].ToLower()
  
    $filter = New-Object Amazon.EC2.Model.Filter
    $filter.Name = "volume-id"
    $filter.Value.Add($volumeId)
    
    $snapshots = get-EC2Snapshot -Filter $filter    
    Write-Output("`nFor volume-id = " + $volumeId)
    foreach ($s in $snapshots)
    {
       $d = ([DateTime]::Now).AddDays(-$daysBack)
       $meetsCriteria = $false
       if ([DateTime]::Compare($d, $s.StartTime) -gt 0)
       {
            $meetsCriteria = $true
       }
       
       $sb = $s.SnapshotId + ", " + $s.StartTime + ", meets criteria for delete? " + $meetsCriteria
       if (!$viewOnly -AND $meetsCriteria) 
       {
           $resp = Remove-EC2Snapshot -SnapshotId $s.SnapshotId
           #Can get RequestId from response for troubleshooting.
           $sb = $sb + ", deleted? yes"
       }
       else {
           $sb = $sb + ", deleted? no"
       }
       Write-Output($sb) 
    }
  }
```

## Understanding Volume Statuses and Transitions<a name="StorageVolumeStatuses"></a>

Each volume has an associated status that tells you at a glance what the health of the volume is\. Most of the time, the status indicates that the volume is functioning normally and that no action is needed on your part\. In some cases, the status indicates a problem with the volume that might or might not require action on your part\. You can find information following to help you decide when you need to act\. You can see volume status on the AWS Storage Gateway console or by using one of the Storage Gateway API operations, for example [DescribeCachediSCSIVolumes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCachediSCSIVolumes.html) or [DescribeStorediSCSIVolumes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeStorediSCSIVolumes.html)\.

**Topics**
+ [Understanding Volume Status](#StorageVolumeStatuses2)
+ [Understanding Attachment Status](#VolumeAttachStatuses)
+ [Understanding Cached Volume Status Transitions](#CachedVolumeStatusTransition)
+ [Understanding Stored Volume Status Transitions](#StorageVolumeStatusTransition)

### Understanding Volume Status<a name="StorageVolumeStatuses2"></a>

 The following table shows volume status on the Storage Gateway console\. Volume status appears in the **Status** column for each storage volume on your gateway\. A volume that is functioning normally has a status of **Available**\. 

In the following table, you can find a description of each storage volume status, and if and when you should act based on each status\. The **Available** status is the normal status of a volume\. A volume should have this status all or most of the time it's in use\. 


| Status | Meaning | 
| --- | --- | 
| <a name="VolumeStatusAVAILABLE"></a><a name="VolumeStatusAVAILABLE.title"></a>Available |  The volume is available for use\. This status is the normal running status for a volume\.  When a **Bootstrapping** phase is completed, the volume returns to **Available** state\. That is, the gateway has synchronized any changes made to the volume since it first entered **Pass Through** status\.  | 
| <a name="VolumeStatusBOOTSTRAPPING"></a><a name="VolumeStatusBOOTSTRAPPING.title"></a>Bootstrapping |  The gateway is synchronizing data locally with a copy of the data stored in AWS\. You typically don't need to take action for this status, because the storage volume automatically sees the **Available** status in most cases\.  The following are scenarios when a volume status is **Bootstrapping**:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/managing-volumes.html)  | 
| <a name="VolumeStatusCREATING"></a><a name="VolumeStatusCREATING.title"></a>Creating |  The volume is currently being created and is not ready for use\. The **Creating** status is transitional\. No action is required\.  | 
| <a name="VolumeStatusDELETING"></a><a name="VolumeStatusDELETING.title"></a>Deleting |  The volume is currently being deleted\. The **Deleting** status is transitional\. No action is required\.  | 
| <a name="VolumeStatusIRRECOVERABLE"></a><a name="VolumeStatusIRRECOVERABLE.title"></a>Irrecoverable |  An error occurred from which the volume cannot recover\. For information on what to do in this situation, see [Troubleshooting volume issues](troubleshoot-volume-issues.md)\.  | 
| <a name="VolumeStatusPASSTHROUGH"></a><a name="VolumeStatusPASSTHROUGH.title"></a>Pass Through |  Data maintained locally is out of sync with data stored in AWS\. Data written to a volume while the volume is in **Pass Through** status remains in the cache until the volume status is **Bootstrapping**\. This data starts to upload to AWS when **Bootstrapping** status begins\.  The **Pass Through** status can occur for several reasons, listed following:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/managing-volumes.html)  | 
| <a name="VolumeStatusRESTORING"></a><a name="VolumeStatusRESTORING.title"></a>Restoring |  The volume is being restored from an existing snapshot\. This status applies only for stored volumes\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. If you restore two storage volumes at the same time, both storage volumes show **Restoring** as their status\. Each storage volume changes to the **Available** status automatically when it is finished being created\. You can read and write to a storage volume and take a snapshot of it while it has the **Restoring** status\.  | 
| Restoring Pass Through |  The volume is being restored from an existing snapshot and has encountered an upload buffer issue\. This status applies only for stored volumes\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. One reason that can cause the **Restoring Pass Through** status is if your gateway has run out of upload buffer space\. Your applications can continue to read from and write data to your storage volumes while they have the **Restoring Pass Through** status\. However, you can't take snapshots of a storage volume during the **Restoring Pass Through** status period\. For information about what action to take when your storage volume has the **Restoring Pass Through** status because upload buffer capacity has been exceeded, see [Troubleshooting volume issues](troubleshoot-volume-issues.md)\.  Infrequently, the **Restoring Pass Through** status can indicate that a disk allocated for an upload buffer has failed\. For information about what action to take in this scenario, see [Troubleshooting volume issues](troubleshoot-volume-issues.md)\.  | 
| <a name="VolumeStatusUPLOADBUFFERNOTCONFIGURED"></a><a name="VolumeStatusUPLOADBUFFERNOTCONFIGURED.title"></a>Upload Buffer Not Configured |  You can't create or use the volume because the gateway doesn't have an upload buffer configured\. For information on how to add upload buffer capacity for volumes in a cached volume setup, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\. For information on how to add upload buffer capacity for volumes in a stored volume setup, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\.  | 

### Understanding Attachment Status<a name="VolumeAttachStatuses"></a>

 You can detach a volume from a gateway or attach it to a gateway by using the Storage Gateway console or API\. The following table shows volume attachment status on the Storage Gateway console\. Volume attachment status appears in the **Attachment status** column for each storage volume on your gateway\. For example, a volume that is detached from a gateway has a status of **Detached**\. For information about how to detach and attach a volume, see [Moving Your Volumes to a Different Gateway](#attach-detach-volume)\.


| Status | Meaning | 
| --- | --- | 
| <a name="VolumeStatusATTACHED"></a><a name="VolumeStatusATTACHED.title"></a>Attached |  The volume is attached to a gateway\.  | 
| <a name="VolumeStatusDETACHED"></a><a name="VolumeStatusDETACHED.title"></a>Detached |  The volume is detached from a gateway\.  | 
| <a name="VolumeStatusDETACHING"></a><a name="VolumeStatusDETACHING.title"></a>Detaching |  The volume is being detached from a gateway\. When you are detaching a volume and the volume doesn't have data on it, you might not see this status\.  | 

### Understanding Cached Volume Status Transitions<a name="CachedVolumeStatusTransition"></a>

Use the following state diagram to understand the most common transitions between statuses for volumes in cached gateways\. You don't need to understand the diagram in detail to use your gateway effectively\. Rather, the diagram provides detailed information if you are interested in knowing more about how volume gateways work\.

The diagram doesn't show the **Upload Buffer Not Configured** status or the **Deleting** status\. Volume states in the diagram appear as green, yellow, and red boxes\. You can interpret the colors as described following\.


| Color | Volume Status | 
| --- | --- | 
| Green | The gateway is operating normally\. The volume status is Available or eventually becomes Available\. | 
| Yellow | The volume has the Pass Through status, which indicates there is a potential issue with the storage volume\. If this status appears because the upload buffer space is filled, then in some cases buffer space becomes available again\. At that point, the storage volume self\-corrects to the Available status\. In other cases, you might have to add more upload buffer space to your gateway to allow the storage volume status to become Available\. For information on how to troubleshoot a case when upload buffer capacity has been exceeded, see [Troubleshooting volume issues](troubleshoot-volume-issues.md)\. For information on how to add upload buffer capacity, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\. | 
| Red | The storage volume has the Irrecoverable status\. In this case, you should delete the volume\. For information on how to do this, see [To remove a volume](#CachedRemovingAStorageVolume)\. | 

In the diagram, a transition between two states is depicted with a labeled line\. For example, the transition from the **Creating** status to the **Available** status is labeled as *Create Basic Volume or Create Volume from Snapshot*\. This transition represents creating a cached volume\. For more information about creating storage volumes, see [Adding a Volume](#ApplicationStorageVolumesCached-Adding)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/VolumeStateDiagramCachedVolume-diagram.png)

**Note**  
The volume status of **Pass Through** appears as yellow in this diagram\. However, this doesn't match the color of this status icon in the **Status** box of the Storage Gateway console\.

### Understanding Stored Volume Status Transitions<a name="StorageVolumeStatusTransition"></a>

Use the following state diagram to understand the most common transitions between statuses for volumes in stored gateways\. You don't need to understand the diagram in detail to use your gateway effectively\. Rather, the diagram provides detailed information if you are interested in understanding more about how volume gateways work\.

The diagram doesn't show the **Upload Buffer Not Configured** status or the **Deleting** status\. Volume states in the diagram appear as green, yellow, and red boxes\. You can interpret the colors as described following\.


| Color | Volume Status | 
| --- | --- | 
| Green | The gateway is operating normally\. The volume status is Available or eventually becomes Available\. | 
| Yellow | When you are creating a storage volume and preserving data, then the path from the Creating status to the Pass Through status occurs if another volume is bootstrapping\. In this case, the volume with the Pass Through status goes to the Bootstrapping status and then to the Available status when the first volume is finished bootstrapping\. Other than the specific scenario mentioned, yellow \(Pass Through status\) indicates that there is a potential issue with the storage volume, the most common one being an upload buffer issue\. If upload buffer capacity has been exceeded, then in some cases buffer space becomes available again\. At that point, the storage volume self\-corrects to the Available status\. In other cases, you might have to add more upload buffer capacity to your gateway to return the storage volume to the Available status\. For information on how to troubleshoot a case when upload buffer capacity has been exceeded, see [Troubleshooting volume issues](troubleshoot-volume-issues.md)\. For information on how to add upload buffer capacity, see [Determining the Size of Upload Buffer to Allocate](ManagingLocalStorage-common.md#CachedLocalDiskUploadBufferSizing-common)\. | 
| Red | The storage volume has the Irrecoverable status\. In this case, you should delete the volume\. For information on how to do this, see [Deleting a Volume](#ApplicationStorageVolumesCached-Removing)\. | 

In the following diagram, a transition between two states is depicted with a labeled line\. For example, the transition from the **Creating** status to the **Available** status is labeled as *Create Basic Volume*\. This transition represents creating a storage volume without preserving data or creating the volume from a snapshot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/VolumeStateDiagram-diagram.png)

**Note**  
The volume status of **Pass Through** appears as yellow in this diagram\. However, this doesn't match the color of this status icon in the **Status** box of the Storage Gateway console\.