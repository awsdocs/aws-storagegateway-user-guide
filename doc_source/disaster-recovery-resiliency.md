# Resilience in AWS Storage Gateway<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS global infrastructure, Storage Gateway offers several features to help support your data resiliency and backup needs:
+ Use VMware vSphere High Availability \(VMware HA\) to help protect storage workloads against hardware, hypervisor, or network failures\. For more information, see [Using VMware vSphere High Availability with AWS Storage Gateway](Performance.md#vmware-ha)\.
+ Use AWS Backup to back up your volumes\. For more information, see [Backing Up Your Volumes](backing-up-volumes.md)\.
+ Clone your volume from a recovery point\. For more information, see [Cloning a Volume](managing-volumes.md#clone-volume)\.
+ Archive virtual tapes in Amazon S3 Glacier\. For more information, see [Archiving Virtual Tapes](managing-gateway-vtl.md#archiving-tapes-vtl)\.