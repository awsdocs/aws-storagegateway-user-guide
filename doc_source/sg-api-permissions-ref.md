# Storage Gateway API permissions: Actions, resources, and conditions reference<a name="sg-api-permissions-ref"></a>

When you set up [access control](UsingIAMWithStorageGateway.md#access-control) and write permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each Storage Gateway API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your Storage Gateway policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `storagegateway:` prefix followed by the API operation name \(for example, `storagegateway:ActivateGateway`\)\. For each Storage Gateway action, you can specify a wildcard character \(\*\) as the resource\.

Use the scroll bars to see the rest of the table\.

For a list of Storage Gateway resources with their ARN formats, see [Storage Gateway resources and operations](managing-access-overview.md#access-control-specify-sg-actions)\.


**Storage Gateway API and required permissions for actions**  

| Storage Gateway API operations | Required permissions \(API actions\) | Resources | 
| --- | --- | --- | 
|  [ActivateGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ActivateGateway.html)  |  `storagegateway:ActivateGateway`  | \* | 
|   [AddCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddCache.html)  |  `storagegateway:AddCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddTagsToResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddTagsToResource.html)  |  `storagegateway:AddTagsToResource`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  or  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id ` or  `arn:aws:storagegateway:region:account-id:tape/tapebarcode`  | 
|   [AddUploadBuffer](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddUploadBuffer.html)  |  `storagegateway:AddUploadBuffer`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [AddWorkingStorage](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_AddWorkingStorage.html)  |  `storagegateway:AddWorkingStorage`  | `arn:aws:storagegateway:region:account-id:gateway/gateway-id` | 
|   [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html)  |  `storagegateway:CreateNFSFileShare`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [CreateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateSMBFileShare.html)   |  `storagegateway:CreateSMBFileShare`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [DeleteBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteBandwidthRateLimit.html)  |  `storagegateway:DeleteBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DeleteChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteChapCredentials.html)  |  `storagegateway:DeleteChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DeleteFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteFileShare.html)  |  `storagegateway:DeleteFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|  [DeleteGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteGateway.html)  |  `storagegateway:DeleteGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DescribeAvailabilityMonitorTest](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeAvailabilityMonitorTest.html)   |  `storagegateway:DescribeAvailabilityMonitorTest`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [DescribeBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeBandwidthRateLimit.html)  |  `storagegateway:DescribeBandwidthRateLimit`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeCache.html)  |  `storagegateway:DescribeCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeChapCredentials.html)  |  `storagegateway:DescribeChapCredentials`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/target/iSCSItarget | 
|  [DescribeGatewayInformation](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeGatewayInformation.html)  |  `storagegateway:DescribeGatewayInformation`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [DescribeMaintenanceStartTime](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeMaintenanceStartTime.html)  |  `storagegateway:DescribeMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DescribeNFSFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeNFSFileShares.html)  |  `storagegateway:DescribeNFSFileShares`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [DescribeSMBFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSMBFileShares.html)   |  `storagegateway:DescribeSMBFileShares`  | arn:aws:storagegateway:region:account\-id:share/share\-id | 
|   [DescribeSMBSettings](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSMBSettings.html)   |  `storagegateway:DescribeSMBSettings`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [DescribeWorkingStorage](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeWorkingStorage.html)  |  `storagegateway:DescribeWorkingStorage`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DisableGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DisableGateway.html)  |  `storagegateway:DisableGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [DisassociateFileSystem](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DisassociateFileSystem)  |  `storagegateway:API_DisassociateFileSystem`  |  `arn:aws:fsx:region:account-id:file-system/filesystem-id`  | 
|   [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html)   |  `storagegateway:JoinDomain`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [ListFileShares](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListFileShares.html)  |  `storagegateway:ListFileShares`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListLocalDisks](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListLocalDisks.html)  |  `storagegateway:ListLocalDisks`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [ListTagsForResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ListTagsForResource.html)  |  `storagegateway:ListTagsForResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode | 
|  [RefreshCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RefreshCache.html)  |  `storagegateway:RefreshCache`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [RemoveTagsFromResource](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_RemoveTagsFromResource.html)  |  `storagegateway:RemoveTagsFromResource`  |  arn:aws:storagegateway:region:account\-id:gateway/gateway\-id or arn:aws:storagegateway:region:account\-id:gateway/gateway\-id/volume/volume\-id or arn:aws:storagegateway:region:account\-id:tape/tapebarcode  | 
|  [ResetCache](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ResetCache.html)  |  `storagegateway:ResetCache`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [SetLocalConsolePassword](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_SetLocalConsolePassword.html)   |  `storagegateway:SetLocalConsolePassword`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [SetSMBGuestPassword](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_SetSMBGuestPassword.html)   |  `storagegateway:SetSMBGuestPassword`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [ShutdownGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ShutdownGateway.html)  |  `storagegateway:ShutdownGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [StartGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartGateway.html)  |  `storagegateway:StartGateway`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|   [StartAvailabilityMonitorTest](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html)   |  `storagegateway:StartAvailabilityMonitorTest`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateBandwidthRateLimit](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateBandwidthRateLimit.html)  |  `storagegateway:UpdateBandwidthRateLimit`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|   [UpdateChapCredentials](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateChapCredentials.html)  | 
|  [UpdateGatewayInformation](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewayInformation.html)  |  `storagegateway:UpdateGatewayInformation`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 
|  [UpdateGatewaySoftwareNow](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateGatewaySoftwareNow.html)  |  `storagegateway:UpdateGatewaySoftwareNow`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateMaintenanceStartTime](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateMaintenanceStartTime.html)  |  `storagegateway:UpdateMaintenanceStartTime`  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
|  [UpdateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateNFSFileShare.html)  |  `storagegateway:UpdateNFSFileShare`  |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
|   [UpdateSMBFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBFileShare.html)   |  `storagegateway:UpdateSMBFileShare`  | arn:aws:storagegateway:region:account\-id:share/share\-id | 
|   [UpdateSMBSecurityStrategy](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_UpdateSMBSecurityStrategy.html)   |  `storagegateway:UpdateSMBSecurityStrategy`  | arn:aws:storagegateway:region:account\-id:gateway/gateway\-id | 

Related topics
+ [Access control](UsingIAMWithStorageGateway.md#access-control)
+ [Customer managed policy examples](using-identity-based-policies.md#customer-managed-policies)