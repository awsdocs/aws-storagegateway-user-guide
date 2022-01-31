--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Storage Gateway API Permissions: Actions, Resources, and Conditions Reference<a name="sg-api-permissions-ref"></a>

When you set up [access control](UsingIAMWithStorageGateway.md#access-control) and write permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each Storage Gateway API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your Storage Gateway policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `storagegateway:` prefix followed by the API operation name \(for example, `storagegateway:ActivateGateway`\)\. For each Storage Gateway action, you can specify a wildcard character \(\*\) as the resource\.

Use the scroll bars to see the rest of the table\.

For a list of Storage Gateway resources with their ARN formats, see [Storage Gateway Resources and Operations](managing-access-overview.md#access-control-specify-sg-actions)\.


**Storage Gateway API and Required Permissions for Actions**  

| Storage Gateway API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [ActivateGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ActivateGateway.html)  |  `storagegateway:ActivateGateway`  | \* | 
|   [AddCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddCache.html)  |  `storagegateway:AddCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddTagsToResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddTagsToResource.html)  |  `storagegateway:AddTagsToResource`  | `arn:aws:storagegateway:region:account-id:gateway/gateway-id` or `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id `or `arn:aws:storagegateway:region:account-id:tape/tapebarcode` AWS now requires users who wish to include a list of tags to have the storagegateway:AddTagsToResource permission\. | 
|   [AddUploadBuffer](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddUploadBuffer.html)  |  `storagegateway:AddUploadBuffer`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddWorkingStorage](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddWorkingStorage.html)  |  `storagegateway:AddWorkingStorage`  | `arn:aws:storagegateway:region:account-id:gateway/gateway-id` | 
|   [AssignTapePool](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AssignTapePool.html)   |  `storagegateway:AssignTapePool`  | `arn:aws:storagegateway:region:account-id:tape/tapebarcode` | 
|   [AttachVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AttachVolume.html)   |  `storagegateway:AttachVolume`  | `arn:aws:storagegateway:region:account-id:gateway/gateway-id` | 
|   [CancelArchival](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CancelArchival.html)  |  `storagegateway:CancelArchival`  | `arn:aws:storagegateway:region:account-id:tape/tapebarcode` | 
|   [CancelRetrieval](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CancelRetrieval.html)  |  `storagegateway:CancelRetrieval`  | arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|   [CreateCachediSCSIVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateCachediSCSIVolume.html)  |  `storagegateway:CreateCachediSCSIVolume`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html)  |  `storagegateway:CreateNFSFileShare`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html)   |  `storagegateway:CreateSMBFileShare`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateSnapshot](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSnapshot.html)  |  `storagegateway:CreateSnapshot`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|   [CreateSnapshotFromVolumeRecoveryPoint](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSnapshotFromVolumeRecoveryPoint.html)  |  `storagegateway:CreateSnapshotFromVolumeRecoveryPoint`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|   [CreateStorediSCSIVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateStorediSCSIVolume.html)  |  `storagegateway:CreateStorediSCSIVolume`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [CreateTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html)  |  `storagegateway:CreateTapes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [CreateTapeWithBarcode](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapeWithBarcode.html)   |  `storagegateway:CreateTapeWithBarcode`  | With default pool or null pool gateways: `arn:aws:storagegateway:region:account-id:gateway/gateway-id` With custom pool: `arn:aws:storagegateway:region:account-id:gateway/gateway-id` `arn:aws:storagegateway:region:account-id:pool/pool-id`  | 
|   [DeleteBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteBandwidthRateLimit.html)  |  `storagegateway:DeleteBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteChapCredentials.html)  |  `storagegateway:DeleteChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DeleteFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html)  |  `storagegateway:DeleteFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|  [DeleteGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteGateway.html)  |  `storagegateway:DeleteGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteSnapshotSchedule](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteSnapshotSchedule.html)  |  `storagegateway:DeleteSnapshotSchedule`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DeleteTape](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteTape.html)  |  `storagegateway:DeleteTape`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteTapeArchive](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteTapeArchive.html)  |  `storagegateway:DeleteTapeArchive`  |  `*`  | 
|  [DeleteVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteVolume.html)  |  `storagegateway:DeleteVolume`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|   [DescribeAvailabilityMonitorTest](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeAvailabilityMonitorTest.html)   |  `storagegateway:DescribeAvailabilityMonitorTest`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [DescribeBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeBandwidthRateLimit.html)  |  `storagegateway:DescribeBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCache.html)  |  `storagegateway:DescribeCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeCachediSCSIVolumes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCachediSCSIVolumes.html)  |  `storagegateway:DescribeCachediSCSIVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DescribeChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeChapCredentials.html)  |  `storagegateway:DescribeChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DescribeGatewayInformation](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeGatewayInformation.html)  |  `storagegateway:DescribeGatewayInformation`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DescribeMaintenanceStartTime](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeMaintenanceStartTime.html)  |  `storagegateway:DescribeMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeNFSFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeNFSFileShares.html)  |  `storagegateway:DescribeNFSFileShares`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [DescribeSMBFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSMBFileShares.html)   |  `storagegateway:DescribeSMBFileShares`  | arn:aws:storagegateway:region:account\-id:share/share\-id | 
|   [DescribeSMBSettings](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSMBSettings.html)   |  `storagegateway:DescribeSMBSettings`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [DescribeSnapshotSchedule](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshotSchedule.html)  |  `storagegateway:DescribeSnapshotSchedule`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id | 
|  [DescribeStorediSCSIVolumes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeStorediSCSIVolumes.html)  |  `storagegateway:DescribeStorediSCSIVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DescribeTapeArchives](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapeArchives.html)  |  `storagegateway:DescribeTapeArchives`  | \* | 
|  [DescribeTapeRecoveryPoints](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapeRecoveryPoints.html)  |  `storagegateway:DescribeTapeRecoveryPoints`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapes.html)  |  `storagegateway:DescribeTapes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeUploadBuffer](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeUploadBuffer.html)  |  `storagegateway:DescribeUploadBuffer`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeVTLDevices](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeVTLDevices.html)  |  `storagegateway:DescribeVTLDevices`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeWorkingStorage](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeWorkingStorage.html)  |  `storagegateway:DescribeWorkingStorage`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DetachVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DetachVolume.html)   |  `storagegateway:DetachVolume`  | `arn:aws:storagegateway:region:account-id:volume/volume-id` | 
|  [DisableGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DisableGateway.html)  |  `storagegateway:DisableGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html)   |  `storagegateway:JoinDomain`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [ListFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListFileShares.html)  |  `storagegateway:ListFileShares`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListGateways](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListGateways.html)  |  `storagegateway:ListGateways`  | \* | 
|  [ListLocalDisks](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListLocalDisks.html)  |  `storagegateway:ListLocalDisks`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListTagsForResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListTagsForResource.html)  |  `storagegateway:ListTagsForResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|  [ListTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListTapes.html)  |  `storagegateway:ListTapes`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id  | 
|   [ListVolumeInitiators](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumeInitiators.html)  |  `storagegateway:ListVolumeInitiators`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id | 
|  [ListVolumeRecoveryPoints](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumeRecoveryPoints.html)  |  `storagegateway:ListVolumeRecoveryPoints`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListVolumes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumes.html)  |  `storagegateway:ListVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [NotifyWhenUploaded](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_NotifyWhenUploaded.html)   |  `storagegateway:NotifyWhenUploaded`  | arn:aws:storagegateway:region:account\-id:share/share\-id | 
|  [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html)  |  `storagegateway:RefreshCache`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [RemoveTagsFromResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RemoveTagsFromResource.html)  |  `storagegateway:RemoveTagsFromResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode  | 
|  [ResetCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ResetCache.html)  |  `storagegateway:ResetCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [RetrieveTapeArchive](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RetrieveTapeArchive.html)  |  `storagegateway:RetrieveTapeArchive`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [RetrieveTapeRecoveryPoint](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RetrieveTapeRecoveryPoint.html)  |  `storagegateway:RetrieveTapeRecoveryPoint`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [SetLocalConsolePassword](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_SetLocalConsolePassword.html)   |  `storagegateway:SetLocalConsolePassword`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [SetSMBGuestPassword](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_SetSMBGuestPassword.html)   |  `storagegateway:SetSMBGuestPassword`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [ShutdownGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ShutdownGateway.html)  |  `storagegateway:ShutdownGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [StartGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartGateway.html)  |  `storagegateway:StartGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [StartAvailabilityMonitorTest](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html)   |  `storagegateway:StartAvailabilityMonitorTest`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)  |  `storagegateway:UpdateBandwidthRateLimit`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [UpdateChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateChapCredentials.html)  |  `storagegateway:UpdateChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [UpdateGatewayInformation](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewayInformation.html)  |  `storagegateway:UpdateGatewayInformation`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateGatewaySoftwareNow](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewaySoftwareNow.html)  |  `storagegateway:UpdateGatewaySoftwareNow`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateMaintenanceStartTime](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateMaintenanceStartTime.html)  |  `storagegateway:UpdateMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html)  |  `storagegateway:UpdateNFSFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html)   |  `storagegateway:UpdateSMBFileShare`  | arn:aws:storagegateway:region:account\-id:share/share\-id | 
|   [UpdateSMBSecurityStrategy](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBSecurityStrategy.html)   |  `storagegateway:UpdateSMBSecurityStrategy`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateSnapshotSchedule](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSnapshotSchedule.html)  |  `storagegateway:UpdateSnapshotSchedule`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [UpdateVTLDeviceType](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateVTLDeviceType.html)  |  `storagegateway:UpdateVTLDeviceType`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/device/vtldevice | 

Related Topics
+ [Access Control](UsingIAMWithStorageGateway.md#access-control)
+ [Customer Managed Policy Examples](using-identity-based-policies.md#customer-managed-policies)