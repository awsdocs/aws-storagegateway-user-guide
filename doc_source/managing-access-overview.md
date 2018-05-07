# Overview of Managing Access Permissions to Your AWS Storage Gateway<a name="managing-access-overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and some services \(such as AWS Lambda\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the IAM User Guide\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [AWS Storage Gateway Resources and Operations](#access-control-specify-sg-actions)
+ [Understanding Resource Ownership](#access-control-owner)
+ [Managing Access to Resources](#access-control-managing-permissions)
+ [Specifying Policy Elements: Actions, Effects, Resources, and Principals](#policy-elements)
+ [Specifying Conditions in a Policy](#specifying-conditions)

## AWS Storage Gateway Resources and Operations<a name="access-control-specify-sg-actions"></a>

In AWS Storage Gateway, the primary resource is a *gateway*\. Storage Gateway also supports the following additional resource types: *file share*, *volume*, * virtual tape*, *iSCSI target*, and *vtl device*\. These are referred to as *subresources* and they don't exist unless they are associated with a gateway\.

These resources and subresources have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following table\.


| Resource Type | ARN Format | 
| --- | --- | 
|  Gateway ARN  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
| File Share ARN |  `arn:aws:storagegateway:region:account-id:share/share-id`  | 
| Volume ARN |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/volume/volume-id`  | 
| Tape ARN |  `arn:aws:storagegateway:region:account-id:tape/tapebarcode`  | 
| Target ARN \( iSCSI target\) |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/target/iSCSItarget`  | 
|  VTL Device ARN  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/device/vtldevice`  | 

**Note**  
AWS Storage Gateway resource IDs are in uppercase\. When you use these resource IDs with the Amazon EC2 API, Amazon EC2 expects resource IDs in lowercase\. You must change your resource ID to lowercase to use it with the EC2 API\. For example, in Storage Gateway the ID for a volume might be `vol-1122AABB`\. When you use this ID with the EC2 API, you must change it to `vol-1122aabb`\. Otherwise, the EC2 API might not behave as expected\.
ARNs for gateways activated prior to September 2, 2015, contain the gateway name instead of the gateway ID\. To obtain the ARN for your gateway, use the `DescribeGatewayInformation` API operation\.

To grant permissions for specific API operations, such as creating a tape, AWS Storage Gateway provides a set of API actions for you to create and manage these resources and subresources\. For a list of API actions, see [Actions](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_Operations.html) in the *AWS Storage Gateway API Reference*\. 

To grant permissions for specific API operations, such as creating a tape, AWS Storage Gateway defines a set of actions that you can specify in a permissions policy to grant permissions for specific API operations\. An API operation can require permissions for more than one action\. For a table showing all the AWS Storage Gateway API actions and the resources they apply to, see [AWS Storage Gateway API Permissions: Actions, Resources, and Conditions Reference](sg-api-permissions-ref.md)\.

## Understanding Resource Ownership<a name="access-control-owner"></a>

A *resource owner* is the AWS account that created the resource\. That is, the resource owner is the AWS account of the *principal entity* \(the root account, an IAM user, or an IAM role\) that authenticates the request that creates the resource\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to activate a gateway, your AWS account is the owner of the resource \(in AWS Storage Gateway, the resource is the gateway\)\.
+ If you create an IAM user in your AWS account and grant permissions to the `ActivateGateway` action to that user, the user can activate a gateway\. However, your AWS account, to which the user belongs, owns the gateway resource\.
+ If you create an IAM role in your AWS account with permissions to activate a gateway, anyone who can assume the role can activate a gateway\. Your AWS account, to which the role belongs, owns the gateway resource\. 

## Managing Access to Resources<a name="access-control-managing-permissions"></a>

A permissions policy describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of AWS Storage Gateway\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What is IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the IAM User Guide\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the IAM User Guide\.

Policies attached to an IAM identity are referred to as *identity\-based* policies \(IAM polices\) and policies attached to a resource are referred to as *resource\-based* policies\. AWS Storage Gateway supports only identity\-based policies \(IAM policies\)\. 

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#identity-based-policies)
+ [Resource\-Based Policies](#resource-based-policies)

### Identity\-Based Policies \(IAM Policies\)<a name="identity-based-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – An account administrator can use a permissions policy that is associated with a particular user to grant permissions for that user to create an AWS Storage Gateway resource, such as a gateway, volume, or tape\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\. 

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following is an example policy that grants permissions to all `List*` actions on all resources\. This action is a read\-only action\. Thus, the policy doesn't allow the user to change the state of the resources\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowAllListActionsOnAllResources",
            "Effect": "Allow",
            "Action": [
                "storagegateway:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about using identity\-based policies with AWS Storage Gateway, see [Using Identity\-Based Policies \(IAM Policies\) for AWS Storage Gateway](using-identity-based-policies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

### Resource\-Based Policies<a name="resource-based-policies"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. AWS Storage Gateway doesn't support resource\-based policies\. 

## Specifying Policy Elements: Actions, Effects, Resources, and Principals<a name="policy-elements"></a>

For each AWS Storage Gateway resource \(see [AWS Storage Gateway API Permissions: Actions, Resources, and Conditions Reference](sg-api-permissions-ref.md)\), the service defines a set of API operations \(see [Actions](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_Operations.html)\)\. To grant permissions for these API operations, AWS Storage Gateway defines a set of actions that you can specify in a policy\. For example, for the AWS Storage Gateway gateway resource, the following actions are defined: `ActivateGateway`, `DeleteGateway`, and `DescribeGatewayInformation`\. Note that, performing an API operation can require permissions for more than one action\.

The following are the most basic policy elements:
+ **Resource** – In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource to which the policy applies\. For AWS Storage Gateway resources, you always use the wildcard character `(*)` in IAM policies\. For more information, see [AWS Storage Gateway Resources and Operations](#access-control-specify-sg-actions)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, depending on the specified `Effect`, the `storagegateway:ActivateGateway` permission allows or denies the user permissions to perform the AWS Storage Gateway `ActivateGateway` operation\.
+ **Effect** – You specify the effect when the user requests the specific action—this can be either allow or deny\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. AWS Storage Gateway doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the AWS Storage Gateway API actions, see [AWS Storage Gateway API Permissions: Actions, Resources, and Conditions Reference](sg-api-permissions-ref.md)\.

## Specifying Conditions in a Policy<a name="specifying-conditions"></a>

When you grant permissions, you can use the IAM policy language to specify the conditions when a policy should take effect when granting permissions\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are no condition keys specific to Storage Gateway\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 