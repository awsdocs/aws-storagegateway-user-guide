# AWS Storage Gateway Limits<a name="resource-gateway-limits"></a>

In this topic, you can find information about file share, volume, tape, configuration, and performance limits for Storage Gateway\.

**Topics**
+ [Limits For File Shares](#resource-file-limits)
+ [Limits for Volumes](#resource-volume-limits)
+ [Limits for Tapes](#resource-tape-limits)
+ [Configuration and Performance Limits](#performance-limits)
+ [Recommended Local Disk Sizes For Your Gateway](#disk-sizes)

## Limits For File Shares<a name="resource-file-limits"></a>

The following table lists limits for file shares\.


| Description | File Gateway | 
| --- | --- | 
| Maximum number of file shares per Amazon S3 bucket\. There is a one\-to\-one mapping between a file share and an S3 bucket | 1 | 
| Maximum number of file shares per gateway | 10 | 
| The maximum size of an individual file, which is the maximum size of an individual object in Amazon S3  If you write a file larger than 5 TB, you get a "file too large" error message and only the first 5 TB of the file is uploaded\.   | 5 TB | 

## Limits for Volumes<a name="resource-volume-limits"></a>

The following table lists limits for volumes\.


| Description | Cached Volumes | Stored Volumes | 
| --- | --- | --- | 
| Maximum size of a volume  If you create a snapshot from a cached volume that is more than 16 TiB in size, you cannot restore it to an Amazon Elastic Block Store \(Amazon EBS\) volume; however, it can be restored to a Storage Gateway volume\.   | 32 TiB | 16 TiB | 
| Maximum number of volumes for a gateway | 32 | 32 | 
| Total size of all volumes for a gateway | 1,024 TiB | 512 TiB | 

## Limits for Tapes<a name="resource-tape-limits"></a>

The following table lists limits for tapes\.


| Description | Tape Gateway | 
| --- | --- | 
| Minimum size of a virtual tape | 100 GiB | 
| Maximum size of a virtual tape | 2\.5 TiB | 
| Maximum number of virtual tapes for a virtual tape library \(VTL\) | 1,500  | 
| Total size of all tapes in a virtual tape library \(VTL\) | 1 PiB | 
| Maximum number of virtual tapes in archive | No limit | 
| Total size of all tapes in a archive | No limit | 

## Configuration and Performance Limits<a name="performance-limits"></a>

The following table lists limits for configuration and performance\.


| Description | File Gateway | Cached Volumes | Stored Volumes | Tape Gateway | 
| --- | --- | --- | --- | --- | 
| Maximum upload rate  The maximum upload rate was achieved by using 100 percent sequential write operations and 256 KB I/Os\. Depending on your I/O mix and network conditions, the actual rate might be lower\.   | – | 120 MB/s | 120 MB/s | 120 MB/s | 
| Maximum download rate | – | 20 MB/s | 20 MB/s | 20 MB/s | 

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