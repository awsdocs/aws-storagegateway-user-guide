# What Is AWS Storage Gateway?<a name="WhatIsStorageGateway"></a>

AWS Storage Gateway connects an on\-premises software appliance with cloud\-based storage to provide seamless integration with data security features between your on\-premises IT environment and the AWS storage infrastructure\. You can use the service to store data in the AWS Cloud for scalable and cost\-effective storage that helps maintain data security\. 

AWS Storage Gateway offers file\-based, volume\-based, and tape\-based storage solutions:

**File Gateway** – A file gateway supports a file interface into Amazon Simple Storage Service \(Amazon S3\) and combines a service and a virtual software appliance\. By using this combination, you can store and retrieve objects in Amazon S3 using industry\-standard file protocols such as Network File System \(NFS\)\. The software appliance, or gateway, is deployed into your on\-premises environment as a virtual machine \(VM\) running on VMware ESXi or Microsoft Hyper\-V hypervisor\. The gateway provides access to objects in S3 as files on an NFS mount point\. With a file gateway, you can do the following: 

+ You can store and retrieve files directly using the NFS version 3 or 4\.1 protocol\.

+ You can access your data directly in Amazon S3 from any AWS Cloud application or service\.

+ You can manage your Amazon S3 data using lifecycle policies, cross\-region replication, and versioning\. You can think of a file gateway as an NFS mount on S3\.

A file gateway simplifies file storage in Amazon S3, integrates to existing applications through industry\-standard file system protocols, and provides a cost\-effective alternative to on\-premises storage\. It also provides low\-latency access to data through transparent local caching\. A file gateway manages data transfer to and from AWS, buffers applications from network congestion, optimizes and streams data in parallel, and manages bandwidth consumption\. File gateways integrate with AWS services, for example with the following:

+ Common access management using AWS Identity and Access Management \(IAM\) 

+ Encryption using AWS Key Management Service \(AWS KMS\)

+ Monitoring using Amazon CloudWatch \(CloudWatch\)

+ Audit using AWS CloudTrail \(CloudTrail\)

+ Operations using the AWS Management Console and AWS Command Line Interface \(AWS CLI\)

+ Billing and cost management

**Volume Gateway** – A volume gateway provides cloud\-backed storage volumes that you can mount as Internet Small Computer System Interface \(iSCSI\) devices from your on\-premises application servers\. The gateway supports the following volume configurations:

+ **Cached volumes** – You store your data in Amazon Simple Storage Service \(Amazon S3\) and retain a copy of frequently accessed data subsets locally\. Cached volumes offer a substantial cost savings on primary storage and minimize the need to scale your storage on\-premises\. You also retain low\-latency access to your frequently accessed data\.

+ **Stored volumes** – If you need low\-latency access to your entire dataset, first configure your on\-premises gateway to store all your data locally\. Then asynchronously back up point\-in\-time snapshots of this data to Amazon S3\. This configuration provides durable and inexpensive offsite backups that you can recover to your local data center or Amazon EC2\. For example, if you need replacement capacity for disaster recovery, you can recover the backups to Amazon EC2\. 

**Tape Gateway** – With a tape gateway, you can cost\-effectively and durably archive backup data in Amazon Glacier\. A tape gateway provides a virtual tape infrastructure that scales seamlessly with your business needs and eliminates the operational burden of provisioning, scaling, and maintaining a physical tape infrastructure\. 

You can run AWS Storage Gateway either on\-premises as a VM appliance, or in AWS as an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. You deploy your gateway on an EC2 instance to provision iSCSI storage volumes in AWS\. Gateways hosted on EC2 instances can be used for disaster recovery, data mirroring, and providing storage for applications hosted on Amazon EC2\.

For an architectural overview, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\. To see the wide range of use cases that AWS Storage Gateway helps make possible, see the [AWS Storage Gateway detail page](http://aws.amazon.com/storagegateway/#pricing)\. 

To get started with Storage Gateway, see the following\.


+ [Are You a First\-Time AWS Storage Gateway User?](#are-you-first-time-user)
+ [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)
+ [AWS Storage Gateway Pricing](#Pricing)
+ [Plan Your Storage Gateway Deployment](#planning-gateway-deployment)

## Are You a First\-Time AWS Storage Gateway User?<a name="are-you-first-time-user"></a>

In the following documentation, you can find a Getting Started section that covers setup information common to all gateways and also gateway\-specific setup sections\. The Getting Started section shows you how to deploy, activate, and configure storage for a gateway\. The management section shows you how to manage your gateway and resources:

+ [Creating a File Gateway](create-file-gateway.md) provides instructions on how to create and use a file gateway\. It shows you how to create a file share, map your drive to an Amazon S3 bucket, and upload files and folders to Amazon S3\. 

+ [Creating a Volume Gateway](create-volume-gateway-volume.md) describes how to create and use a volume gateway\. It shows you how to create storage volumes and back up data to the volumes\. 

+ [Creating a Tape Gateway](create-tape-gateway.md) provides instructions on how to create and use a tape gateway\. It shows you how to back up data to virtual tapes and archive the tapes\.

+ [Managing Your Gateway](managing-gateway-common.md) describes how to perform management tasks for all gateway types and resources\.

In this guide, you can primarily find how to work with gateway operations by using the AWS Management Console\. If you want to perform these operations programmatically, see the *[AWS Storage Gateway API Reference](http://docs.aws.amazon.com/storagegateway/latest/APIReference/)\.* 

## AWS Storage Gateway Pricing<a name="Pricing"></a>

For current information about pricing, see [Pricing](http://aws.amazon.com/storagegateway/pricing) on the AWS Storage Gateway details page\.

## Plan Your Storage Gateway Deployment<a name="planning-gateway-deployment"></a>

By using the AWS Storage Gateway software appliance, you can connect your existing on\-premises application infrastructure with scalable, cost\-effective AWS cloud storage that provides data security features\.

To deploy Storage Gateway, you first need to decide on the following two things:

1. **Your storage solution** – Choose from one of the following storage solutions:

   + **File Gateway** – You can use a file gateway to ingest files to Amazon S3 for use by object\-based workloads and for cost\-effective storage for traditional backup applications\. You can also use it to tier on\-premises file storage to S3\. You can cost\-effectively and durably store and retrieve your on\-premises objects in Amazon S3 using industry\-standard file protocols\. 

   + **Volume Gateway** – Using volume gateways, you can create storage volumes in the AWS Cloud\. Your on\-premises applications can access these as Internet Small Computer System Interface \(iSCSI\) targets\. There are two options—cached and stored volumes\.

     With cached volumes, you store volume data in AWS, with a small portion of recently accessed data in the cache on\-premises\. This approach enables low\-latency access to your frequently accessed dataset\. It also provides seamless access to your entire dataset stored in AWS\. By using cached volumes, you can scale your storage resource without having to provision additional hardware\.

     With stored volumes, you store the entire set of volume data on\-premises and store periodic point\-in\-time backups \(snapshots\) in AWS\. In this model, your on\-premises storage is primary, delivering low\-latency access to your entire dataset\. AWS storage is the backup that you can restore in the event of a disaster in your data center\.

     For an architectural overview of volume gateways, see [Cached Volumes Architecture](StorageGatewayConcepts.md#storage-gateway-cached-concepts) and [Stored Volumes Architecture](StorageGatewayConcepts.md#storage-gateway-stored-volume-concepts)\.

   + **Tape Gateway** – If you are looking for a cost\-effective, durable, long\-term, offsite alternative for data archiving, deploy a tape gateway\. With its virtual tape library \(VTL\) interface, you can use your existing tape\-based backup software infrastructure to store data on virtual tape cartridges that you create\. For more information, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\. When you archive tapes, you don't worry about managing tapes on your premises and arranging shipments of tapes offsite\. For an architectural overview, see [Tape Gateways](StorageGatewayConcepts.md#storage-gateway-vtl-concepts)\.

1. **Hosting option** – You can run Storage Gateway either on\-premises as a VM appliance, or in AWS as an Amazon EC2 instance\. For more information, see [Requirements](Requirements.md)\. If your data center goes offline and you don't have an available host, you can deploy a gateway on an EC2 instance\. Storage Gateway provides an Amazon Machine Image \(AMI\) that contains the gateway VM image\. 

Additionally, as you configure a host to deploy a gateway software appliance, you need to allocate sufficient storage for the gateway VM\. 

Before you continue to the next step, make sure that you have done the following:

1. For a gateway deployed on\-premises, you chose the type of host, VMware ESXi Hypervisor or Microsoft Hyper\-V\. and set it up\. For more information, see [Requirements](Requirements.md)\. If you deploy the gateway behind a firewall, make sure that ports are accessible to the gateway VM\. For more information, see [Requirements](Requirements.md)\. 

1. For a tape gateway, you have installed client backup software\. For more information, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.