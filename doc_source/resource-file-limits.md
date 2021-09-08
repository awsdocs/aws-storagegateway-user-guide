# Quotas for file shares<a name="resource-file-limits"></a>

The following table lists quotas for file shares\.


| Description | File gateway | 
| --- | --- | 
| Maximum number of file shares per Amazon S3 bucket\. There is a one\-to\-one mapping between a file share and an S3 bucket | 1 | 
| Maximum number of file shares per gateway | 10 | 
| The maximum size of an individual file, which is the maximum size of an individual object in Amazon S3  If you write a file larger than 5 TB, you get a "file too large" error message and only the first 5 TB of the file is uploaded\.   | 5 TB | 
| Maximum path length  Clients are not allowed to create a path exceeding this length, and doing so results in an error\. This limit applies to both protocols supported by file gateways, NFS and SMB\.  | 1024 bytes | 

## Recommended local disk sizes for your gateway<a name="disk-sizes"></a>

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | 
| File gateway | 150 GiB | 64 TiB | — | 

**Note**  
You can configure one or more local drives for your cache up to the maximum capacity\.  
When adding cache to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as a cache\.