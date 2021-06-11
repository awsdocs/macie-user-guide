--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Service\-Linked Roles for Amazon Macie Classic<a name="using-service-linked-roles"></a>

Amazon Macie Classic uses an AWS Identity and Access Management \(IAM\) [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. This service\-linked role is a unique type of IAM role that's linked directly to Macie Classic\. It's predefined by Macie Classic and includes all the permissions that Macie Classic needs to call other AWS services on your behalf\. The defined permissions include the trust policy and the permissions policy, and that permissions policy can't be attached to any other IAM entity\.

Macie Classic uses a service\-linked role in all the AWS Regions where Macie Classic is available\.

## Service\-Linked Role Permissions for Macie Classic<a name="slr-permissions"></a>

Macie Classic uses the service\-linked role named **AWSServiceRoleForAmazonMacie**\. The permissions policy for the role allows Macie Classic to discover, classify, and protect sensitive data in AWS on your behalf\. The role trusts the `macie.amazonaws.com` service to assume the role\.

The role is configured with the following AWS managed permissions policy, named **AmazonMacieServiceRolePolicy**\.

```
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Effect":"Allow",
            "Action":[
                "cloudtrail:DescribeTrails",
                "cloudtrail:GetEventSelectors",
                "cloudtrail:GetTrailStatus",
                "cloudtrail:ListTags",
                "cloudtrail:LookupEvents",
                "iam:ListAccountAliases",
                "organizations:DescribeAccount",
                "organizations:ListAccounts",
                "s3:GetAccountPublicAccessBlock",
                "s3:ListAllMyBuckets",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation",
                "s3:GetBucketLogging",
                "s3:GetBucketPolicy",
                "s3:GetBucketPolicyStatus",
                "s3:GetBucketPublicAccessBlock",
                "s3:GetBucketTagging",
                "s3:GetBucketVersioning",
                "s3:GetBucketWebsite",
                "s3:GetEncryptionConfiguration",
                "s3:GetLifecycleConfiguration",
                "s3:GetReplicationConfiguration",
                "s3:ListBucket",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectTagging"
            ],
            "Resource":"*"
        },
        {
            "Effect":"Allow",
            "Action":[
                "cloudtrail:CreateTrail",
                "cloudtrail:StartLogging",
                "cloudtrail:StopLogging",
                "cloudtrail:UpdateTrail",
                "cloudtrail:DeleteTrail",
                "cloudtrail:PutEventSelectors"
            ],
            "Resource":"arn:aws:cloudtrail:*:*:trail/AWSMacieTrail-DO-NOT-EDIT"
        },
        {
            "Effect":"Allow",
            "Action":[
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:DeleteBucketPolicy",
                "s3:DeleteBucketWebsite",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:DeleteObjectVersion",
                "s3:DeleteObjectVersionTagging",
                "s3:PutBucketPolicy"
            ],
            "Resource":[
                "arn:aws:s3:::awsmacie-*",
                "arn:aws:s3:::awsmacietrail-*",
                "arn:aws:s3:::*-awsmacietrail-*"
            ]
        },
        {
            "Effect":"Allow",
            "Action":[
                "logs:CreateLogGroup"
            ],
            "Resource":[
                "arn:aws:logs:*:*:log-group:/aws/macie/*"
            ]
        },
        {
            "Effect":"Allow",
            "Action":[
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource":[
                "arn:aws:logs:*:*:log-group:/aws/macie/*:log-stream:*"
            ]
        }
    ]
}
```

For details about updates to the **AmazonMacieServiceRolePolicy** policy, see [Updates to AWS Managed Policies for Macie and Macie Classic](security-iam-awsmanpol.md#security-iam-awsmanpol-updates)\.

Amazon Macie Classic and the new Amazon Macie use the same service\-linked role and permissions policy\. \(This helps you move to and use the new Macie\.\) Macie Classic performs all the actions allowed by the policy except `CreateLogGroup`, `CreateLogStream`, `PutLogEvents`, and `DescribeLogStreams`\. Only the new Macie performs those actions on resources, as defined by the policy\.

In addition, Macie Classic doesn’t perform actions on the following resources: `arn:aws:logs:*:*:log-group:/aws/macie/*` and `arn:aws:logs:*:*:log-group:/aws/macie/*:log-stream:*`\. Only the new Macie performs actions on those resources, as defined by the policy\.

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a Service\-Linked Role for Macie Classic<a name="create-slr"></a>

You don't need to manually create the **AWSServiceRoleForAmazonMacie** role\. Macie Classic creates this role on your behalf as follows:
+ **Macie Classic administrator account** — The **AWSServiceRoleForAmazonMacie** role was created for you when you enabled Macie Classic for the first time\.
+ **Member accounts** — The **AWSServiceRoleForAmazonMacie** role was created for you when the Macie Classic administrator account associated the member account with Macie Classic\. The service\-linked role that was created for the Macie Classic administrator account doesn't apply to Macie Classic member accounts\.

Macie Classic can create the service\-linked role on your behalf only if you have the required permissions\. To grant the required permissions to an IAM entity, such as a user, group, or role, attach the **AmazonMacieFullAccess** policy\. For more information, see [Predefined AWS Managed Policies for Macie Classic](macie-access-control.md#managed-policies) and [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

You can also create the **AWSServiceRoleForAmazonMacie** role manually\. For more information, see [Creating a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

### Legacy Roles for Macie Classic<a name="macie-legacy-roles"></a>

If you used Macie Classic before June 21, 2018, when it began supporting service\-linked roles, the IAM roles that grant Macie Classic access to call other AWS services on your behalf already exist in your account \(Macie Classic administrator or member\)\. These roles are **AmazonMacieServiceRole** and **AmazonMacieSetupRole**\. They were created when you launched either the Macie Classic AWS CloudFormation template for an administrator account or the Macie Classic AWS CloudFormation template for a member account as part of setting up Macie Classic\.

The service\-linked role replaces these previously created IAM roles \(in administrator and member accounts\)\. The previously created roles were not deleted, but they're no longer used to grant Macie Classic permission to call other services on your behalf\. You can use the IAM console to delete the previously created roles\.

## Editing a Service\-Linked Role for Macie Classic<a name="edit-slr"></a>

After you create a service\-linked role, you can't change the name of the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a Service\-Linked Role for Macie Classic<a name="delete-slr"></a>

If you no longer need to use Amazon Macie Classic \(or the new Amazon Macie\), we recommend that you delete the **AWSServiceRoleForAmazonMacie** role\.

For a Macie Classic administrator account, you can delete the service\-linked role only after disabling Macie Classic\. This ensures that you can't inadvertently remove permissions to access Macie Classic resources\. For member accounts, the administrator account must first disassociate them from Macie Classic\. For more information, see [Disabling Amazon Macie Classic and Deleting Collected Metadata](macie-disable.md)\.

When you disable Macie Classic, the **AWSServiceRoleForAmazonMacie** role is not deleted\. If you move to the new Macie, the new Macie uses the existing **AWSServiceRoleForAmazonMacie** role\.

You can use the IAM console, the IAM CLI, or the IAM API to delete the **AWSServiceRoleForAmazonMacie** role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.