# AWS Storage Gateway API Permissions: Actions, Resources, and Conditions Reference<a name="sg-api-permissions-ref"></a>

 When you are setting up [Access Control](UsingIAMWithStorageGateway.md#access-control) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each AWS Storage Gateway API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your AWS Storage Gateway policies to express conditions\. For a complete list of AWS\-wide keys, see [Available keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `storagegateway:` prefix followed by the API operation name \(for example, `storagegateway:ActivateGateway`\)\. For each AWS Storage Gateway action, you can specify a wildcard character \(\*\) as the resource\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.

For a list of Storage Gateway resources with the ARN format, see [AWS Storage Gateway Resources and Operations](managing-access-overview.md#access-control-specify-sg-actions)\.


**AWS Storage Gateway API and Required Permissions for Actions**  

| Storage Gateway API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [ActivateGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ActivateGateway.html)  |  `storagegateway:ActivateGateway`  | \* | 
|   [AddCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddCache.html)  |  `storagegateway:AddCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddTagsToResource](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddTagsToResource.html)  |  `storagegateway:AddTagsToResource`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  or  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id ` or  `arn:aws:storagegateway:region:account-id:tape/tapebarcode` | 
|   [AddUploadBuffer](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddUploadBuffer.html)  |  `storagegateway:AddUploadBuffer`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddWorkingStorage](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddWorkingStorage.html)  |  `storagegateway:AddWorkingStorage`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CancelArchival](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CancelArchival.html)  |  `storagegateway:CancelArchival`  | arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|   [CancelRetrieval](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CancelRetrieval.html)  |  `storagegateway:CancelRetrieval`  | arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|   [CreateCachediSCSIVolume](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateCachediSCSIVolume.html)  |  `storagegateway:CreateCachediSCSIVolume`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateNFSFileShare](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html)  |  `storagegateway:CreateNFSFileShare`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateSnapshot](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSnapshot.html)  |  `storagegateway:CreateSnapshot`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|   [CreateSnapshotFromVolumeRecoveryPoint](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSnapshotFromVolumeRecoveryPoint.html)  |  `storagegateway:CreateSnapshotFromVolumeRecoveryPoint`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|   [CreateStorediSCSIVolume](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateStorediSCSIVolume.html)  |  `storagegateway:CreateStorediSCSIVolume`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [CreateTapes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html)  |  `storagegateway:CreateTapes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DeleteBandwidthRateLimit](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteBandwidthRateLimit.html)  |  `storagegateway:DeleteBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteChapCredentials](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteChapCredentials.html)  |  `storagegateway:DeleteChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DeleteFileShare](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html)  |  `storagegateway:DeleteFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|  [DeleteGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteGateway.html)  |  `storagegateway:DeleteGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteSnapshotSchedule](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteSnapshotSchedule.html)  |  `storagegateway:DeleteSnapshotSchedule`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DeleteTape](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteTape.html)  |  `storagegateway:DeleteTape`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteTapeArchive](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteTapeArchive.html)  |  `storagegateway:DeleteTapeArchive`  | `*` | 
|  [DeleteVolume](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteVolume.html)  |  `storagegateway:DeleteVolume`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DescribeBandwidthRateLimit](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeBandwidthRateLimit.html)  |  `storagegateway:DescribeBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCache.html)  |  `storagegateway:DescribeCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeCachediSCSIVolumes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCachediSCSIVolumes.html)  |  `storagegateway:DescribeCachediSCSIVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DescribeChapCredentials](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeChapCredentials.html)  |  `storagegateway:DescribeChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DescribeGatewayInformation](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeGatewayInformation.html)  |  `storagegateway:DescribeGatewayInformation`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DescribeMaintenanceStartTime](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeMaintenanceStartTime.html)  |  `storagegateway:DescribeMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeNFSFileShares](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeNFSFileShares.html)  |  `storagegateway:DescribeNFSFileShares`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|  [DescribeSnapshotSchedule](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshotSchedule.html)  |  `storagegateway:DescribeSnapshotSchedule`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id | 
|  [DescribeStorediSCSIVolumes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeStorediSCSIVolumes.html)  |  `storagegateway:DescribeStorediSCSIVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [DescribeTapeArchives](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapeArchives.html)  |  `storagegateway:DescribeTapeArchives`  | \* | 
|  [DescribeTapeRecoveryPoints](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapeRecoveryPoints.html)  |  `storagegateway:DescribeTapeRecoveryPoints`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeTapes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeTapes.html)  |  `storagegateway:DescribeTapes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeUploadBuffer](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeUploadBuffer.html)  |  `storagegateway:DescribeUploadBuffer`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeVTLDevices](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeVTLDevices.html)  |  `storagegateway:DescribeVTLDevices`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeWorkingStorage](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeWorkingStorage.html)  |  `storagegateway:DescribeWorkingStorage`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DisableGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DisableGateway.html)  |  `storagegateway:DisableGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListFileShares](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListFileShares.html)  |  `storagegateway:ListFileShares`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListGateways](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListGateways.html)  |  `storagegateway:ListGateways`  | \* | 
|  [ListLocalDisks](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListLocalDisks.html)  |  `storagegateway:ListLocalDisks`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListTagsForResource](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListTagsForResource.html)  |  `storagegateway:ListTagsForResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|  [ListTapes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListTapes.html)  |  `storagegateway:ListTapes`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id  | 
|   [ListVolumeInitiators](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumeInitiators.html)  |  `storagegateway:ListVolumeInitiators`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id | 
|  [ListVolumeRecoveryPoints](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumeRecoveryPoints.html)  |  `storagegateway:ListVolumeRecoveryPoints`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListVolumes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListVolumes.html)  |  `storagegateway:ListVolumes`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [RefreshCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html)  |  `storagegateway:RefreshCache`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [RemoveTagsFromResource](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RemoveTagsFromResource.html)  |  `storagegateway:RemoveTagsFromResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode  | 
|  [ResetCache](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ResetCache.html)  |  `storagegateway:ResetCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [RetrieveTapeArchive](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RetrieveTapeArchive.html)  |  `storagegateway:RetrieveTapeArchive`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [RetrieveTapeRecoveryPoint](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RetrieveTapeRecoveryPoint.html)  |  `storagegateway:RetrieveTapeRecoveryPoint`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ShutdownGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ShutdownGateway.html)  |  `storagegateway:ShutdownGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [StartGateway](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartGateway.html)  |  `storagegateway:StartGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateBandwidthRateLimit](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)  |  `storagegateway:UpdateBandwidthRateLimit`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [UpdateChapCredentials](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateChapCredentials.html)  |  `storagegateway:UpdateChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [UpdateGatewayInformation](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewayInformation.html)  |  `storagegateway:UpdateGatewayInformation`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateGatewaySoftwareNow](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewaySoftwareNow.html)  |  `storagegateway:UpdateGatewaySoftwareNow`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateMaintenanceStartTime](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateMaintenanceStartTime.html)  |  `storagegateway:UpdateMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateNFSFileShare](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html)  |  `storagegateway:UpdateNFSFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|  [UpdateSnapshotSchedule](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSnapshotSchedule.html)  |  `storagegateway:UpdateSnapshotSchedule`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
|  [UpdateVTLDeviceType](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateVTLDeviceType.html)  |  `storagegateway:UpdateVTLDeviceType`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/device/vtldevice | 

Related Topics

+ [Access Control](UsingIAMWithStorageGateway.md#access-control)

+ [Customer Managed Policy Examples](using-identity-based-policies.md#customer-managed-policies)