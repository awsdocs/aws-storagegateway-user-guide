# Using Your Backup Software to Test Your Gateway Setup<a name="GettingStartedTestGatewayVTL"></a>

You test your tape gateway setup by performing the following tasks using your backup application:

1. Configure the backup application to detect your storage devices\.
**Note**  
To improve I/O performance, we recommend setting the block size of the tape drives in your backup application to 1 MB For more information, see [Use a Larger Block Size for Tape Drives](Performance.md#block-size)\.

1. Back up data to a tape\.

1. Archive the tape\.

1. Retrieve the tape from the archive\.

1. Restore data from the tape\. 

To test your setup, use a compatible backup application, as described following\.

**Note**  
Unless otherwise stated, all backup applications were qualified on Microsoft Windows\. 

**Topics**
+ [Testing Your Setup by Using Arcserve Backup r17\.0](backup-arcserve.md)
+ [Testing Your Setup by Using Bacula Enterprise](backup-bacula.md)
+ [Testing Your Setup by Using Commvault](backup-commvault.md)
+ [Testing Your Setup by Using Dell EMC NetWorker](backup-emc.md)
+ [Testing Your Setup by Using IBM Spectrum Protect](backup-tsm.md)
+ [Testing Your Setup by Using Micro Focus \(HPE\) Data Protector](backup-hpdataprotector.md)
+ [Testing Your Setup by Using Microsoft System Center Data Protection Manager](backup-DPM.md)
+ [Testing Your Setup by Using NovaStor DataCenter/Network](backup-novastor.md)
+ [Testing Your Setup by Using Quest NetVault Backup](backup-netvault.md)
+ [Testing Your Setup by Using Veeam Backup & Replication](backup-Veeam.md)
+ [Testing Your Setup by Using Veritas Backup Exec](backup-BackupExec.md)
+ [Testing Your Setup by Using Veritas NetBackup](backup_netbackup-vtl.md)

For more information about compatible backup applications, see [Supported Third\-Party Backup Applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.