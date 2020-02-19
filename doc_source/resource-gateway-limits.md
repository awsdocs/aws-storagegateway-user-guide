# AWS Storage Gateway Quotas<a name="resource-gateway-limits"></a>

In this topic, you can find information about file share, volume, tape quotas, configuration, and performance limits for Storage Gateway\.

**Topics**
+ [Quotas for File Shares](#resource-file-limits)
+ [Quotas for Volumes](#resource-volume-limits)
+ [Quotas for Tapes](#resource-tape-limits)
+ [Recommended Local Disk Sizes For Your Gateway](#disk-sizes)

## Quotas for File Shares<a name="resource-file-limits"></a>

The following table lists quotas for file shares\.


| Description | File Gateway | 
| --- | --- | 
| Maximum number of file shares per Amazon S3 bucket\. There is a one\-to\-one mapping between a file share and an S3 bucket | 1 | 
| Maximum number of file shares per gateway | 10 | 
| The maximum size of an individual file, which is the maximum size of an individual object in Amazon S3  If you write a file larger than 5 TB, you get a "file too large" error message and only the first 5 TB of the file is uploaded\.   | 5 TB | 
| Maximum path length Clients are not allowed to create a path exceeding this length, and doing so results in an error\. This limit applies to both protocols supported by file gateways, NFS and SMB\.  | 1024 bytes | 

## Quotas for Volumes<a name="resource-volume-limits"></a>

The following table lists quotas for volumes\.


| Description | Cached Volumes | Stored Volumes | 
| --- | --- | --- | 
| Maximum size of a volume  If you create a snapshot from a cached volume that is more than 16 TiB in size, you can restore it to a Storage Gateway volume but not to an Amazon Elastic Block Store \(Amazon EBS\) volume\.   | 32 TiB | 16 TiB | 
| Maximum number of volumes per gateway | 32 | 32 | 
| Total size of all volumes for a gateway | 1,024 TiB | 512 TiB | 

## Quotas for Tapes<a name="resource-tape-limits"></a>

The following table lists quotas for tapes\.


| Description | Tape Gateway | 
| --- | --- | 
| Minimum size of a virtual tape | 100 GiB | 
| Maximum size of a virtual tape | 5 TiB | 
| Maximum number of virtual tapes for a virtual tape library \(VTL\) | 1,500  | 
| Total size of all tapes in a virtual tape library \(VTL\) | 1 PiB | 
| Maximum number of virtual tapes in archive | No limit | 
| Total size of all tapes in a archive | No limit | 

## Recommended Local Disk Sizes For Your Gateway<a name="disk-sizes"></a>

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