# How AWS Storage Gateway Works \(Architecture\)<a name="StorageGatewayConcepts"></a>

Following, you can find an architectural overview of the available AWS Storage Gateway solutions\.


+ [File Gateways](#file-gateway-concepts)
+ [Volume Gateways](#volume-gateway-concepts)
+ [Tape Gateways](#storage-gateway-vtl-concepts)

## File Gateways<a name="file-gateway-concepts"></a>

To use file gateway storage, you start by downloading a VM image for the file storage gateway\. You then activate it from the AWS Management Console or the Storage Gateway API\. 

After the VM is activated, you configure the S3 buckets that the gateway later exposes as file systems through NFS v3 or v4\.1\. Files written to NFS become objects in Amazon S3, with the path as the key\. There is a one\-to\-one mapping between files and objects, and the gateway asynchronously updates the objects in Amazon S3 as you change the files\. Existing objects in the bucket appear as files in the file system, and the key becomes the path\. Objects are encrypted with server\-side encryption with Amazon S3–managed encryption keys \(SSE\-S3\)\. All data transfer is done through HTTPS\. 

The service optimizes data transfer between the gateway and AWS using multipart parallel uploads or byte\-range downloads, to better use the available bandwidth\. As with cached volumes, a local cache is maintained to provide low latency access to the recently accessed data and reduce data egress charges\. CloudWatch metrics provide insight into resource use on the VM and data transfer to and from AWS\. CloudTrail tracks all API calls\. 

With file gateway storage, you can do such tasks as ingesting cloud workloads to S3, performing backup and archive, and tiering storage to the AWS Cloud\. The following diagram provides an overview of file storage deployment for Storage Gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/file-gateway-concepts-diagram.png)

## Volume Gateways<a name="volume-gateway-concepts"></a>

For volume gateways, you can use either cached volumes or stored volumes\.


+ [Cached Volumes Architecture](#storage-gateway-cached-concepts)
+ [Stored Volumes Architecture](#storage-gateway-stored-volume-concepts)

### Cached Volumes Architecture<a name="storage-gateway-cached-concepts"></a>

By using cached volumes, you can use Amazon S3 as your primary data storage, while retaining frequently accessed data locally in your storage gateway\. Cached volumes minimize the need to scale your on\-premises storage infrastructure, while still providing your applications with low\-latency access to their frequently accessed data\. You can create storage volumes up to 32 TiB in size and attach to them as iSCSI devices from your on\-premises application servers\. Your gateway stores data that you write to these volumes in Amazon S3 and retains recently read data in your on\-premises storage gateway's cache and upload buffer storage\. 

Cached volumes can range from 1 GiB to 32 TiB in size and must be rounded to the nearest GiB\. Each gateway configured for cached volumes can support up to 32 volumes for a total maximum storage volume of 1,024 TiB \(1 PiB\)\.

In the cached volumes solution, AWS Storage Gateway stores all your on\-premises application data in a storage volume in Amazon S3\. The following diagram provides an overview of the cached volumes deployment\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/aws-storage-gateway-cached-diagram.png)

After you install the Storage Gateway software appliance—the VM—on a host in your data center and activate it, you use the AWS Management Console to provision storage volumes backed by Amazon S3\. You can also provision storage volumes programmatically using the AWS Storage Gateway API or the AWS SDK libraries\. You then mount these storage volumes to your on\-premises application servers as iSCSI devices\. 

You also allocate disks on\-premises for the VM\. These on\-premises disks serve the following purposes:

+ **Disks for use by the gateway as cache storage** – As your applications write data to the storage volumes in AWS, the gateway first stores the data on the on\-premises disks used for cache storage\. Then the gateway uploads the data to Amazon S3\. The cache storage acts as the on\-premises durable store for data that is waiting to upload to Amazon S3 from the upload buffer\. 

  The cache storage also lets the gateway store your application's recently accessed data on\-premises for low\-latency access\. If your application requests data, the gateway first checks the cache storage for the data before checking Amazon S3\. 

  You can use the following guidelines to determine the amount of disk space to allocate for cache storage\. Generally, you should allocate at least 20 percent of your existing file store size as cache storage\. Cache storage should also be larger than the upload buffer\. This guideline helps make sure that cache storage is large enough to persistently hold all data in the upload buffer that has not yet been uploaded to Amazon S3\. 

+ **Disks for use by the gateway as the upload buffer** – To prepare for upload to Amazon S3, your gateway also stores incoming data in a staging area, referred to as an *upload buffer\.* Your gateway uploads this buffer data over an encrypted Secure Sockets Layer \(SSL\) connection to AWS, where it is stored encrypted in Amazon S3\. 

You can take incremental backups, called *snapshots*, of your storage volumes in Amazon S3\. These point\-in\-time snapshots are also stored in Amazon S3 as Amazon EBS snapshots\. When you take a new snapshot, only the data that has changed since your last snapshot is stored\. You can initiate snapshots on a scheduled or one\-time basis\. When you delete a snapshot, only the data not needed for any other snapshots is removed\. For information about Amazon EBS snapshots, see [Amazon EBS Snapshots](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)\.

You can restore an Amazon EBS snapshot to a gateway storage volume if you need to recover a backup of your data\. Alternatively, for snapshots up to 16 TiB in size, you can use the snapshot as a starting point for a new Amazon EBS volume\. You can then attach this new Amazon EBS volume to an Amazon EC2 instance\.

All gateway data and snapshot data for cached volumes is stored in Amazon S3 and encrypted at rest using server\-side encryption \(SSE\)\. However, you can't access this data with the Amazon S3 API or other tools such as the Amazon S3 Management Console\.

### Stored Volumes Architecture<a name="storage-gateway-stored-volume-concepts"></a>

By using stored volumes, you can store your primary data locally, while asynchronously backing up that data to AWS\. Stored volumes provide your on\-premises applications with low\-latency access to their entire datasets\. At the same time, they provide durable, offsite backups\. You can create storage volumes and mount them as iSCSI devices from your on\-premises application servers\. Data written to your stored volumes is stored on your on\-premises storage hardware\. This data is asynchronously backed up to Amazon S3 as Amazon Elastic Block Store \(Amazon EBS\) snapshots\.

Stored volumes can range from 1 GiB to 16 TiB in size and must be rounded to the nearest GiB\. Each gateway configured for stored volumes can support up to 32 volumes and a total volume storage of 512 TiB \(0\.5 PiB\)\.

With stored volumes, you maintain your volume storage on\-premises in your data center\. That is, you store all your application data on your on\-premises storage hardware\. Then, using features that help maintain data security, the gateway uploads data to the AWS Cloud for cost\-effective backup and rapid disaster recovery\. This solution is ideal if you want to keep data locally on\-premises, because you need to have low\-latency access to all your data, and also to maintain backups in AWS\.

The following diagram provides an overview of the stored volumes deployment\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/aws-storage-gateway-stored-diagram.png)

After you install the AWS Storage Gateway software appliance—the VM—on a host in your data center and activated it, you can create gateway *storage volumes*\. You then map them to on\-premises direct\-attached storage \(DAS\) or storage area network \(SAN\) disks\. You can start with either new disks or disks already holding data\. You can then mount these storage volumes to your on\-premises application servers as iSCSI devices\. As your on\-premises applications write data to and read data from a gateway's storage volume, this data is stored and retrieved from the volume's assigned disk\.

To prepare data for upload to Amazon S3, your gateway also stores incoming data in a staging area, referred to as an *upload buffer*\. You can use on\-premises DAS or SAN disks for working storage\. Your gateway uploads data from the upload buffer over an encrypted Secure Sockets Layer \(SSL\) connection to the AWS Storage Gateway service running in the AWS Cloud\. The service then stores the data encrypted in Amazon S3\. 

You can take incremental backups, called *snapshots*, of your storage volumes\. The gateway stores these snapshots in Amazon S3 as Amazon EBS snapshots\. When you take a new snapshot, only the data that has changed since your last snapshot is stored\. You can initiate snapshots on a scheduled or one\-time basis\. When you delete a snapshot, only the data not needed for any other snapshot is removed\. 

You can restore an Amazon EBS snapshot to an on\-premises gateway storage volume if you need to recover a backup of your data\. You can also use the snapshot as a starting point for a new Amazon EBS volume, which you can then attach to an Amazon EC2 instance\. 

## Tape Gateways<a name="storage-gateway-vtl-concepts"></a>

Tape Gateway offers a durable, cost\-effective solution to archive your data in the AWS Cloud\. With its virtual tape library \(VTL\) interface, you use your existing tape\-based backup infrastructure to store data on virtual tape cartridges that you create on your tape gateway\. Each tape gateway is preconfigured with a media changer and tape drives\. These are available to your existing client backup applications as iSCSI devices\. You add tape cartridges as you need to archive your data\. 

The following diagram provides an overview of tape gateway deployment\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Gateway-VTL-Architecture2-diagram.png)

The diagram identifies the following tape gateway components:

+ **Virtual tape** – A virtual tape is like a physical tape cartridge\. However, virtual tape data is stored in the AWS Cloud\. Like physical tapes, virtual tapes can be blank or can have data written on them\. You can create virtual tapes either by using the Storage Gateway console or programmatically by using the Storage Gateway API\. Each gateway can contain up to 1500 tapes or up to 1 PiB of total tape data at a time\. The size of each virtual tape, which you can configure when you create the tape, is between 100 GiB and 2\.5 TiB\. 

+ **Virtual tape library \(VTL\)** – A VTL is like a physical tape library available on\-premises with robotic arms and tape drives\. Your VTL includes the collection of stored virtual tapes\. Each tape gateway comes with one VTL\.

  The virtual tapes that you create appear in your gateway's VTL\. Tapes in the VTL are backed up by Amazon S3\. As your backup software writes data to the gateway, the gateway stores data locally and then asynchronously uploads it to virtual tapes in your VTL—that is, Amazon S3\.

  + **Tape drive** – A VTL tape drive is analogous to a physical tape drive that can perform I/O and seek operations on a tape\. Each VTL comes with a set of 10 tape drives, which are available to your backup application as iSCSI devices\. 

  + **Media changer** – A VTL media changer is analogous to a robot that moves tapes around in a physical tape library's storage slots and tape drives\. Each VTL comes with one media changer, which is available to your backup application as an iSCSI device\.

+ **Archive** – Archive is analogous to an offsite tape holding facility\. You can archive tapes from your gateway's VTL to the archive\. If needed, you can retrieve tapes from the archive back to your gateway's VTL\.

  + **Archiving tapes** – When your backup software ejects a tape, your gateway moves the tape to the archive for long\-term storage\. The archive is located in the AWS Region in which you activated the gateway\. Tapes in the archive are stored in Amazon Glacier, an extremely low\-cost storage service for data archiving and backup\. For more information, see [Amazon Glacier](http://docs.aws.amazon.com/amazonglacier/latest/dev/introduction.html)\.

  + **Retrieving tapes** – You can't read archived tapes directly\. To read an archived tape, you must first retrieve it to your tape gateway either by using the Storage Gateway console or by using the Storage Gateway API\. A retrieved tape is available in your VTL in about three to five hours after you start retrieval\. 

After you deploy and activate a tape gateway, you mount the virtual tape drives and media changer on your on\-premises application servers as iSCSI devices\. You create virtual tapes as needed\. Then you use your existing backup software application to write data to the virtual tapes\. The media changer loads and unloads the virtual tapes into the virtual tape drives for read and write operations\.

### Allocating Local Disks for the Gateway VM<a name="local-disks-vtl-gateway-architecture"></a>

Your gateway VM needs local disks, which you allocate for the following purposes:

+ **Cache storage** – The cache storage acts as the durable store for data that is waiting to upload to Amazon S3 from the upload buffer\. 

  If your application reads data from a virtual tape, the gateway saves the data to the cache storage\. The gateway stores recently accessed data in the cache storage for low\-latency access\. If your application requests tape data, the gateway first checks the cache storage for the data before downloading the data from AWS\.

+ **Upload buffer** – The upload buffer provides a staging area for the gateway before it uploads the data to a virtual tape\. The upload buffer is also critical for creating recovery points that you can use to recover tapes from unexpected failures\. For more information, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\.

As your backup application writes data to your gateway, the gateway copies data to both the cache storage and the upload buffer\. It then acknowledges completion of the write operation to your backup application\. 

For guidelines on the amount of disk space to allocate for the cache storage and upload buffer, see [Deciding the Amount of Local Disk Storage](ManagingLocalStorage-common.md#decide-local-disks-and-sizes)\. 