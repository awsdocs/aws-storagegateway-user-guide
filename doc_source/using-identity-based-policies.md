# Using Identity\-Based Policies \(IAM Policies\) for AWS Storage Gateway<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your AWS Storage Gateway resources\. For more information, see [Overview of Managing Access Permissions to Your AWS Storage Gateway](managing-access-overview.md)\. 

The sections in this topic cover the following:
+ [Permissions Required to Use the Storage Gateway Console](#additional-console-required-permissions)
+ [AWS Managed Policies for AWS Storage Gateway](#access-policy-examples-aws-managed)
+ [Customer Managed Policy Examples](#customer-managed-policies)

The following shows an example of a permissions policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsSpecifiedActionsOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "storagegateway:ActivateGateway",
                "storagegateway:ListGateways"
            ],
            "Resource": "arn:aws:storagegateway:us-west-2:account-id:gateway/*"
        },
        {
            "Sid": "AllowsSpecifiedEC2ActionsOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot"
            ],
            "Resource": "*"
        }
    ]
}
```

The policy has two statements \(note the `Action` and `Resource` elements in both the statements\):
+ The first statement grants permissions for two Storage Gateway actions \(`storagegateway:ActivateGateway` and `storagegateway:ListGateways`\) on a gateway resource using the *Amazon Resource Name \(ARN\)* for the gateway\. The ARN specifies a wildcard character \(\*\) because you don't know the gateway ID until after you create a gateway\. 
**Note**  
ARNs uniquely identify AWS resources\. For more information, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS General Reference*\.

  The wildcard character \(\*\) at the end of the gateway ARN means that this statement can match any gateway ID\. In this case, the statement allows the `storagegateway:ActivateGateway` and `storagegateway:ListGateways` actions on any gateway in the specified region, `us-west-2`, and the specified ID identifies the account that is owner of the gateway resource\. For information about how to use a wildcard character \(\*\) in a policy, see [Example 2: Allow Read\-Only Access to a Gateway](#sg-example2)\.

  To limit permissions for a particular action to a specific gateway only, create a separate statement for that action in the policy and specify the gateway ID in that statement\. 

   
+ The second statement grants permissions for the `ec2:DescribeSnapshots` and `ec2:DeleteSnapshot` actions\. These Amazon Elastic Compute Cloud \(Amazon EC2\) actions require permissions because snapshots generated from AWS Storage Gateway are stored in Amazon Elastic Block Store \(Amazon EBS\) and managed as Amazon EC2 resources, and thus they require corresponding EC2 actions\. For more information, see [Actions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_Operations.html) in the *Amazon EC2 API Reference*\. Because these Amazon EC2 actions don't support resource\-level permissions, the policy specifies the wildcard character \(\*\) as the `Resource` value instead of specifying a gateway ARN\. 

For a table showing all of the AWS Storage Gateway API actions and the resources that they apply to, see [AWS Storage Gateway API Permissions: Actions, Resources, and Conditions Reference](sg-api-permissions-ref.md)\. 

## Permissions Required to Use the Storage Gateway Console<a name="additional-console-required-permissions"></a>

To use the Storage Gateway console, you need to grant read\-only permissions\. If you plan to describe snapshots, you also need to grant permissions for additional actions as shown in the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsSpecifiedEC2ActionOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Resource": "*"
        }
    ]
}
```

This additional permission is required because the Amazon EBS snapshots generated from AWS Storage Gateway are managed as Amazon EC2 resources\. 

To set up the minimum permissions required to navigate the Storage Gateway console, see [Example 2: Allow Read\-Only Access to a Gateway](#sg-example2)\.

## AWS Managed Policies for AWS Storage Gateway<a name="access-policy-examples-aws-managed"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information about AWS managed policies, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to Storage Gateway:
+ **AWSStorageGatewayReadOnlyAccess** – Grants read\-only access to AWS Storage Gateway resources\. 
+ **AWSStorageGatewayFullAccess** – Grants full access to AWS Storage Gateway resources\. 

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for AWS Storage Gateway API actions\. You can attach these custom policies to the IAM users or groups that require those permissions\.

## Customer Managed Policy Examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for various Storage Gateway actions\. These policies work when you are using AWS SDKs and the AWS CLI\. When you are using the console, you need to grant additional permissions specific to the console, which is discussed in [Permissions Required to Use the Storage Gateway Console](#additional-console-required-permissions)\.

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

**Topics**
+ [Example 1: Allow Any AWS Storage Gateway Actions on All Gateways](#sg-example1)
+ [Example 2: Allow Read\-Only Access to a Gateway](#sg-example2)
+ [Example 3: Allow Access to a Specific Gateway](#sg-example3)
+ [Example 4: Allow a User to Access a Specific Volume](#sg-example4)
+ [Example 5: Allow All Actions on Gateways with a Specific Prefix](#sg-example5)

### Example 1: Allow Any AWS Storage Gateway Actions on All Gateways<a name="sg-example1"></a>

The following policy allows a user to perform all the AWS Storage Gateway actions\. The policy also allows the user to perform Amazon EC2 actions \([DescribeSnapshots](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshots.html) and [DeleteSnapshot](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteSnapshot.html)\) on the Amazon EBS snapshots generated from AWS Storage Gateway\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsAllAWSStorageGatewayActions",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsSpecifiedEC2Actions",
            "Action": [
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 2: Allow Read\-Only Access to a Gateway<a name="sg-example2"></a>

The following policy allows all `List*` and `Describe*` actions on all resources\. Note that these actions are read\-only actions\. Thus, the policy doesn't allow the user to change the state of any resources—that is, the policy doesn't allow the user to perform actions such as `DeleteGateway`, `ActivateGateway`, and `ShutdownGateway`\.

The policy also allows the `DescribeSnapshots` Amazon EC2 action\. For more information, see [DescribeSnapshots](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshots.html) in the *Amazon EC2 API Reference*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowReadOnlyAccessToAllGateways",
            "Action": [
                "storagegateway:List*",
                "storagegateway:Describe*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsUserToDescribeSnapshotsOnAllGateways",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

In the preceding policy, instead of a using a wildcard character \(\*\), you can scope resources covered by the policy to a specific gateway, as shown in the following example\. The policy then allows the actions only on the specific gateway\.

```
"Resource": [
              "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/", 
              "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
```

Within a gateway, you can further restrict the scope of the resources to only the gateway volumes, as shown in the following example: 

```
"Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/*" 
```

### Example 3: Allow Access to a Specific Gateway<a name="sg-example3"></a>

The following policy allows all actions on a specific gateway\. The user is restricted from accessing other gateways you might have deployed\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowReadOnlyAccessToAllGateways",
            "Action": [
                "storagegateway:List*",
                "storagegateway:Describe*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsUserToDescribeSnapshotsOnAllGateways",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsAllActionsOnSpecificGateway",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/",
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
        }
    ]
}
```

The preceding policy works if the user to which the policy is attached uses either the API or an AWS SDK to access the gateway\. However, if the user is going to use the AWS Storage Gateway console, you must also grant permissions to allow the `ListGateways` action, as shown in the following example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsAllActionsOnSpecificGateway",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/",
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
        },
        {
            "Sid": "AllowsUserToUseAWSConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 4: Allow a User to Access a Specific Volume<a name="sg-example4"></a>

The following policy allows a user to perform all actions to a specific volume on a gateway\. Because a user doesn't get any permissions by default, the policy restricts the user to accessing only a specific volume\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GrantsPermissionsToSpecificVolume",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/volume-id"
        },
        {
            "Sid": "GrantsPermissionsToUseStorageGatewayConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

The preceding policy works if the user to whom the policy is attached uses either the API or an AWS SDK to access the volume\. However, if this user is going to use the AWS Storage Gateway console, you must also grant permissions to allow the `ListGateways` action, as shown in the following example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GrantsPermissionsToSpecificVolume",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/volume-id"
        },
        {
            "Sid": "GrantsPermissionsToUseStorageGatewayConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 5: Allow All Actions on Gateways with a Specific Prefix<a name="sg-example5"></a>

The following policy allows a user to perform all AWS Storage Gateway actions on gateways with names that start with `DeptX`\. The policy also allows the `DescribeSnapshots` Amazon EC2 action which is required if you plan to describe snapshots\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsActionsGatewayWithPrefixDeptX",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/DeptX"
        },
        {
            "Sid": "GrantsPermissionsToSpecifiedAction",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

The preceding policy works if the user to whom the policy is attached uses either the API or an AWS SDK to access the gateway\. However, if this user plans to use the AWS Storage Gateway console, you must grant additional permissions as described in [Example 3: Allow Access to a Specific Gateway](#sg-example3)\.