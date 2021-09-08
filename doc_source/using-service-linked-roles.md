# Using service\-linked roles for Storage Gateway<a name="using-service-linked-roles"></a>

Storage Gateway uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Storage Gateway\. Service\-linked roles are predefined by Storage Gateway and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Storage Gateway easier because you don't have to manually add the necessary permissions\. Storage Gateway defines the permissions of its service\-linked roles, and unless defined otherwise, only Storage Gateway can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Storage Gateway<a name="slr-permissions"></a>

Storage Gateway uses the service\-linked role named **AWSServiceRoleForStorageGateway** – AWSServiceRoleForStorageGateway\.

The AWSServiceRoleForStorageGateway service\-linked role trusts the following services to assume the role:
+ `storagegateway.amazonaws.com`

The role permissions policy allows Storage Gateway to complete the following actions on the specified resources:
+ Action: `fsx:ListTagsForResource` on `arn:aws:fsx:*:*:backup/*`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create and edit a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Storage Gateway<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you make an Storage Gateway `AssociateFileSystem` API call in the AWS Management Console, the AWS CLI, or the AWS API, Storage Gateway creates the service\-linked role for you\. 

**Important**  
This service\-linked role can appear in your account if you completed an action in another service that uses the features supported by this role\. Also, if you were using the Storage Gateway service before March 31, 2021, when it began supporting service\-linked roles, then Storage Gateway created the AWSServiceRoleForStorageGateway role in your account\. To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you make an Storage Gateway `AssociateFileSystem` API call, Storage Gateway creates the service\-linked role for you again\. 

You can also use the IAM console to create a service\-linked role with the **AWSServiceRoleForStorageGateway** use case\. In the AWS CLI or the AWS API, create a service\-linked role with the `storagegateway.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

## Editing a service\-linked role for Storage Gateway<a name="edit-slr"></a>

Storage Gateway does not allow you to edit the AWSServiceRoleForStorageGateway service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Storage Gateway<a name="delete-slr"></a>

Storage Gateway doesn't automatically delete the AWSServiceRoleForStorageGateway role\. To delete AWSServiceRoleForStorageGateway role, you need to invoke the `iam:DeleteSLR` API\. If there are no storage gateway resources that depend on the service\-linked\-role then the deletion will succeed, otherwise the deletion will fail\. If you want to delete the service linked role, you need to use IAM APIs `iam:DeleteRole` or `iam:DeleteServiceLinkedRole`\. In this case, you need to use the Storage Gateway APIs to first delete any gateways or file system associations in the account, then delete the service linked role by using `iam:DeleteRole` or `iam:DeleteServiceLinkedRole` API\. When you are deleting the service linked role using IAM, you need to use Storage Gateway `DisassociateFileSystemAssociation` API first to delete all file system associations in the account\. Otherwise, the deletion operation will fail\. 

**Note**  
If the Storage Gateway service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete Storage Gateway resources used by the AWSServiceRoleForStorageGateway**

1. Use our service console, CLI, or API to make a call that cleans up the resources and deletes the role or use the IAM console, CLI, or API to do the deletion\. In this case, you need to use Storage Gateway APIs to first delete any gateways and file\-system\-associations in the account\. 

1. If you use the IAM console, CLI, or API, delete the service\-linked role using IAM `DeleteRole` or `DeleteServiceLinkedRole` API\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForStorageGateway service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for Storage Gateway service\-linked roles<a name="slr-regions"></a>

Storage Gateway supports using service\-linked roles in all of the Regions where the service is available\. For more information, see [AWS service endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.

Storage Gateway does not support using service\-linked roles in every Region where the service is available\. You can use the AWSServiceRoleForStorageGateway role in the following Regions\.


****  

| Region name | Region identity | Support in Storage Gateway | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Osaka\) | ap\-northeast\-3 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| AWS GovCloud \(US\) | us\-gov\-west\-2 | Yes | 