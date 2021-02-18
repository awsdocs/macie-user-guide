--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Moving to the New Amazon Macie<a name="macie-migration"></a>

A new Amazon Macie is now available with significant design improvements and additional features, at a lower price and in most AWS Regions\. We encourage you to take advantage of these improvements and the reduced cost by moving to the new Macie\. With the new Macie, you can use a native AWS console or a comprehensive API to perform tasks such as:
+ Monitor and analyze an unlimited number of Amazon Simple Storage Service \(Amazon S3\) buckets\.
+ Conduct deeper discovery of sensitive data in S3 objects\. The new Macie can analyze more than the first 20 MB of data in an S3 object\.
+ Build and use custom data identifiers to discover sensitive data for particular scenarios\.
+ Tag Macie resources\.
+ Develop and deploy AWS CloudFormation templates for Macie resources\.
+ Centrally manage Macie for multiple accounts\. The new Macie integrates with AWS Organizations, which means that you can manage as many as 5,000 Macie accounts for a single AWS organization\. You can also continue to use native Macie features for managing multiple accounts, which enables you to manage as many as 1,000 member accounts with a single Macie administrator account\.
+ Discover and monitor sensitive data in most AWS Regions, instead of only two Regions\.

To learn about all features and pricing for the new Macie, see [Amazon Macie](https://aws.amazon.com/macie/)\.

If you currently have an Amazon Macie Classic account, we'll continue to support your account\. If you enable a new account, we'll configure the account to use the new Amazon Macie\. To learn how to enable and manage an account for the new Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)\.

This topic walks you through each step of the process to move from Amazon Macie Classic to the new Amazon Macie\.

**Topics**
+ [Overview](#macie-migration-overview)
+ [Before You Begin](#macie-migration-prerequisites)
+ [Step 1: Export Data Classification Results](#macie-migration-procedure)
+ [Step 2: Disable Your Macie Classic Account](#macie-migration-disable-account)
+ [Step 3: Delete Resources and Collected Metadata](#macie-migration-delete-resources)
+ [Step 4: Enable Your New Amazon Macie Account](#macie-migration-next-steps)
+ [Reference: S3 Bucket Policy for Exported Data Classification Results](#macie-migration-results-bucket-policy)

## Overview<a name="macie-migration-overview"></a>

To help you move to the new Amazon Macie, we added an export feature to Macie Classic\. With this feature, you can optionally create copies of \(export\) all the data classification results for your account in the current AWS Region\. Macie Classic adds these copies to an S3 bucket, and it encrypts the data using an AWS Key Management Service \(AWS KMS\) key that you specify\. If your account is a Macie Classic administrator account that has member accounts, the export includes classification results for all associated member accounts\.

When the export starts, your Macie Classic account and its member accounts switch to read\-only mode\. This means that you can't perform tasks such as adding S3 buckets to monitor and creating classification jobs\. In addition, Macie Classic stops performing most activities for your account and its member accounts\. This includes monitoring data, generating classifications, and sending notifications based on Amazon CloudWatch Events\. Macie Classic remains in this mode for the duration of the export\. Depending on how much data you have, the export might take up to two weeks to complete\. You can check the status of the export at any time\.

**Important**  
To ensure continuous data discovery and monitoring for your account and its member accounts, don't start the export process until you enable and configure these accounts in the new Amazon Macie\. To learn how to do this with the new Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)\.  
Note that you will incur costs for both new and existing accounts while the export is in progress\. For each Macie Classic account, we will continue to charge you for CloudTrail event processing and extended data retention\. We will also charge you for each new Macie account that you enable\. To learn about Macie pricing, see [Amazon Macie Pricing](https://aws.amazon.com/macie/pricing/)\.

When the export finishes, Macie Classic also stops processing CloudTrail events for your account and its member accounts\. This means that Macie Classic has stopped performing *all* activities for these accounts\. However, the accounts are still enabled\. 

After the export is complete, you can continue to view your existing data classification results by using the Macie Classic console\. \(You can't view or access these results by using the new Macie\.\) You can also import the results into a log management or visualization tool from the S3 bucket\. In addition, you can continue to view and analyze existing alert data in AWS Security Hub and existing event data in AWS CloudTrail\. 

After you complete the process for a Macie Classic account and its member accounts, you can disable the accounts\. If you use Macie Classic in multiple AWS Regions, repeat this process for each additional Region\. 

## Before You Begin<a name="macie-migration-prerequisites"></a>

Before you begin the export process and move to the new Amazon Macie, ensure that you have the resources that you'll need, and consider the following\.

**Which accounts?**  
If your account is a Macie Classic administrator account that has member accounts, a first step is to determine which accounts you want to move to the new Macie\. Only an administrator account can start the export process and subsequently disable a member account\. In addition, when you start the export process, Macie Classic automatically switches the administrator account and all member accounts to read\-only mode, and it exports the data for the administrator account and all member accounts\.  
If you prefer to move to the new Macie in phases, consider adjusting the current administrator\-member account associations for your organization\. To exclude specific member accounts from a phase, disassociate those accounts from your administrator account\. You can then optionally create a new administrator account for those member accounts\. After you do this, you can start the export process for the new administrator account and its member accounts during a separate phase\. 

**Is continuous monitoring necessary?**  
When the export process starts, Macie Classic stops performing most activities for your account and its member accounts\. This includes monitoring data, generating classifications, and sending notifications based on Amazon CloudWatch Events\. Macie Classic remains in this mode for the duration of the export\. Depending on how much data you have, the export can take up to two weeks to complete\.   
If you want to ensure continuous data discovery and monitoring for your account and its member accounts, you should enable and configure the accounts in the new Amazon Macie before you start the export process\. To learn how to do this, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)\. Note, however, that you will incur costs for the new and existing accounts\. While the export is in progress for a Macie Classic account, we'll continue to charge the account for CloudTrail event processing and extended data retention\. After the export is complete, we'll continue to charge the account for extended data retention until you disable the account\. We'll also charge you for each new Macie account that you enable\. To learn about Macie pricing, see [Amazon Macie Pricing](https://aws.amazon.com/macie/pricing/)\.

**Which S3 bucket?**   
When you start the export process, you can specify where to export your classification results to\. You have two options:  
+ **Use a new S3 bucket that you create** – If you prefer to export the results to a particular S3 bucket that you create, create the bucket, and ensure that the name of the bucket begins with `awsmacie-*`\. \(This allows Macie Classic to access the bucket by using the existing [service\-linked role](using-service-linked-roles.md) for your Macie Classic account\.\) Also, define a bucket policy that allows Macie Classic to create objects in the bucket\. For an example of the bucket policy to use, see [S3 Bucket Policy for Exported Data Classification Results](#macie-migration-results-bucket-policy) later in this topic\.
+ **Use a new S3 bucket that Macie Classic creates** – If you don't specify an S3 bucket, Macie Classic automatically creates a new bucket for you\. \(To do this, it uses the existing [service\-linked role](using-service-linked-roles.md) for your Macie Classic account\.\) To help you find the bucket, it includes `awsmacie-classification-export` in the bucket name\. When it creates the bucket, Macie Classic also applies a bucket policy that allows it to write data to the bucket only if the bucket owner has full control of the bucket's objects\. To see the policy, refer to [S3 Bucket Policy for Exported Data Classification Results](#macie-migration-results-bucket-policy) later in this topic\.
Note that the new Amazon Macie also provides options for storing classification results in S3 buckets\. However, we don't recommend using the same bucket to store classification results for both versions of Macie\. Macie Classic and the new Macie use different schemas for data classification results\.

**Which encryption key?**  
When you start the export process, you have to specify the AWS Key Management Service \(AWS KMS\) key that you want Macie Classic to use to encrypt the exported data\. Therefore, it's a good idea to determine which key you want to use before you start the export, and to note the key's ARN\. You'll need to enter the ARN when you start the export process\. Also, verify that the key's policy allows Macie Classic to perform the `Encrypt` and `GenerateDataKey*` actions\. For example:  

```
{
    "Version":"2012-10-17",
    "Id":"key-policy-id",
    "Statement":[
        {
            "Sid":"Allow Macie Classic to use the key",
            "Effect":"Allow",
            "Principal":{
                "Service":"macie.amazonaws.com"
            },
            "Action":[
                "kms:Encrypt",
                "kms:GenerateDataKey*"
            ],
            "Resource":"*"
        }
    ]
}
```

## Step 1: Export Data Classification Results<a name="macie-migration-procedure"></a>

The first step in moving to the new Amazon Macie is to optionally export the existing data classification results for your account\. If your account is a Macie Classic administrator account, this includes the results for all of its associated member accounts\. 

When you export data classification results, Macie Classic copies the results data to one or more objects in an S3 bucket\. It encrypts the data using an AWS KMS key that you specify\. The number of objects varies depending on how much data you have\.

Each object contains JSON\-formatted data for a batch of classification results\. The data is grouped by calendar day and uses the Macie Classic schema for data classification results\. This means that the data maps closely to the groups and fields in the Macie Classic **Dashboard**\. For descriptions of these groups and fields, see [Viewing Data and Activity that Amazon Macie Classic Monitors](macie-dashboard.md)\.  Each object is compressed and stored as a gzip file\.

In addition to these objects, Macie Classic creates an empty `JOB_START_TOKEN` object in the bucket\. This object indicates that the export started\. It also creates an empty `JOB_END_TOKEN` object\. This object indicates that the export is complete\. You can ignore these objects in any queries or subsequent analysis of the data\.

**To export data classification results**

1. Open the Amazon Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/)\.

1. By using the AWS Region selector in the upper\-right corner of the page, switch to the Region in which you use Macie Classic—**US East \(N\. Virginia\)** or **US West \(Oregon\)**\.

1. In the navigation pane, choose **Macie Classic**\.

1. If your account is a Macie Classic administrator account that has member accounts, disassociate any member accounts that you don't want to include in the export process\.

1. Remove all the S3 resources that you integrated with Macie Classic\. To do this, choose **Integrations** in the navigation pane, and then clear all the current selections for data sources to monitor\.

1. In the banner at the top of the console, choose **Export data**\. If the banner is hidden, reload the page in your browser\.

1. For **KMS key**, enter the Amazon Resource Name \(ARN\) of the AWS KMS key that you want to use to encrypt the exported data\.

1. \(Optional\) For **Bucket name**, enter the name of the S3 bucket that you want to store the exported data in\. If you don't enter a bucket name, Macie Classic automatically creates a bucket for you, and it defines a [bucket policy](#macie-migration-results-bucket-policy) for the bucket\.

1. When you finish entering the export settings, choose **Export data** to start the export\. Macie Classic then verifies that it can create or access the S3 bucket for the exported data\. It also verifies that it can use the AWS KMS key that you specified\. If it can do these things, it begins copying your classification results to the bucket\. 

1. To check the status of the export at any time, repeat steps 1 through 3 to return to the Macie Classic console\. The banner at the top of the console displays the status of the export\. When the export is complete, the banner displays **The export is complete**\. 

   If an issue occurs and Macie Classic can't complete the export, the banner displays **Export failed**\. To display details about the issue, choose **Export failed**\. Depending on the nature of the issue, try to export the data again or contact AWS Support for assistance\.

1. When the export is complete, navigate to the S3 bucket that contains the exported data, and then verify the results\. The number and nature of the results should match the data that appears in the Macie Classic **Dashboard**\.

Repeat the preceding steps for each additional Macie Classic administrator account \(and its member accounts\) and AWS Region\.

## Step 2: Disable Your Macie Classic Account<a name="macie-migration-disable-account"></a>

After the export is complete and you verify the results, you can disable your Macie Classic account\.

**Warning**  
When you disable a Macie Classic account, the following occurs:  
Macie Classic loses access to resources for the Macie Classic administrator account and all of its member accounts\.
You and all other users of the administrator account and its member accounts lose access to the Macie Classic console\.
Macie Classic deletes all the metadata that it collected while monitoring data for the administrator account and all of its member accounts\. Within 90 days of disabling Macie Classic, all of this metadata is expired and removed from Macie Classic system backups\.

Note that Macie Classic doesn't delete data that it generated and stored in other AWS services for you\. This includes existing alert data in AWS Security Hub, event data in AWS CloudTrail, and logs and exported classification results in Amazon S3\.

**To disable your Macie Classic account**

1. Open the Amazon Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/)\.

1. By using the AWS Region selector in the upper\-right corner of the page, switch to the Region in which you want to disable your Macie Classic account—**US East \(N\. Virginia\)** or **US West \(Oregon\)**\.

1. In the navigation pane, choose **Macie Classic**\.

1. In the banner at the top of the console, choose **Disable Macie Classic**\.

1. In the window that appears, review the information about the consequences of disabling your Macie Classic account\. If you're sure that you want to disable your account and its member accounts, select the check boxes, and then choose **Disable Macie Classic**\.

## Step 3: Delete Resources and Collected Metadata<a name="macie-migration-delete-resources"></a>

When you disable a Macie Classic account, the service doesn't delete any data or resources that it created, used, or stored in other AWS services for the account\. It deletes only the data that it collected and stored directly for the account\. 

As a best practice and to avoid unnecessary costs, we recommend that you assess the following resources and data after you disable your Macie Classic account:
+ **AWS CloudTrail data events** – Macie Classic created a trail to enable Amazon S3 data events for the buckets that it monitored\. The new Amazon Macie uses a different architecture and doesn't require you to enable Amazon S3 data events\. If you don't need this logging anymore, you should delete the trail that Macie Classic created\. This prevents further AWS CloudTrail billing charges for the trail\. You can also archive or remove the log data that's stored in the S3 bucket\.
+ **Amazon CloudWatch Events** – Macie Classic doesn't delete CloudWatch Events that it generated for your account\. Although the new Macie also publishes events to CloudWatch, the event data will be specific to your new Macie account\. Therefore, you might decide to delete events for the Macie Classic account that you disabled\. 
+ **Legacy IAM roles** – If you used Macie Classic before June 21, 2018 \(when it began supporting service\-linked roles\), your AWS account has two legacy AWS Identity and Access Management \(IAM\) roles that you don't need anymore\. These roles are **AmazonMacieServiceRole** and **AmazonMacieSetupRole**\. These roles allowed Macie Classic to call other AWS services on your behalf\. The new Macie doesn't use these roles\. You can, therefore, consider deleting them\.

If you started using Macie Classic after June 21, 2018, the service created an **AWSServiceRoleForAmazonMacie** service\-level role on your behalf\. This role allowed Macie Classic to discover and monitor sensitive data on your behalf\. The new Amazon Macie also uses this service\-level role to perform similar tasks\. For this reason, we recommend that you keep this role for use with your new Macie account\.

## Step 4: Enable Your New Amazon Macie Account<a name="macie-migration-next-steps"></a>

When you're ready to start using the new Amazon Macie, sign in to your AWS account, navigate to the Amazon Macie console, and enable a new Macie account\. To learn about features and pricing for the new Macie, see [Amazon Macie](https://aws.amazon.com/macie/)\.

**To enable a new Macie account**

1. Sign in to the AWS Management Console and open the Amazon Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/)\.

1. Choose **Get started**\.

1. Choose **Enable Macie**\.

Within minutes, the new Macie generates an inventory of the S3 buckets for your account, and begins monitoring the buckets for security and access control\.

**Next Steps**

To ensure continued data classification with your new Macie account, immediate next steps are:

**Configure a repository for your sensitive data discovery results**  
In the new Macie, you create and run sensitive data discovery jobs to analyze and report sensitive data in S3 buckets\. When Macie runs a job, it reports any sensitive data that it discovers as a *sensitive data finding*, which is a detailed report of the sensitive data that Macie found\. Macie also creates *sensitive data discovery results*, which are detailed analysis records for S3 objects that the job analyzes or attempts to analyze\. This includes objects that don't contain sensitive data and, therefore, don't produce a finding\.   
When you configure a repository to store your discovery results, you ensure long\-term access and storage of these results for your account\. For more information, see [Storing and retaining sensitive data discovery results](https://docs.aws.amazon.com/macie/latest/user/discovery-results-repository-s3.html) in the *Amazon Macie User Guide*\.

**Create a sensitive data discovery job**  
After you configure a repository for your discovery results, create a job that runs periodically on a daily, weekly, or monthly basis\. To avoid unnecessary costs and duplicated classification data, you can configure the job to analyze only those objects that were created or modified after a certain point in time, such as when you started the export process in Macie Classic\. You can do this in two ways when you configure the scope of the job:  
+ **Skip objects that were created before the job** – With this configuration, the first run of the job analyzes only those objects that are created after you finish creating the job\. Each subsequent run analyzes only those objects that are created after the preceding run\. To use this configuration, clear the **Include existing objects** check box under **Scheduled job**\. 
+ **Skip objects that were created or last modified before a certain time** – With this configuration, each run of the job skips any objects that were created or modified before a date and time that you specify\. To use this configuration, expand the **Additional settings** section\. For **Object criteria**, choose **Last modified**\. In the **To** boxes, enter the latest creation or modification date and time for the objects to skip, and then choose **Exclude**\. 
For more information, see [Running sensitive data discovery jobs](https://docs.aws.amazon.com/macie/latest/user/discovery-jobs.html) in the *Amazon Macie User Guide*\.

For additional next steps and information about configuring and managing your account, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)\. The guide also provides information about managing multiple accounts by using AWS Organizations or sending Macie membership invitations\.

## Reference: S3 Bucket Policy for Exported Data Classification Results<a name="macie-migration-results-bucket-policy"></a>

As discussed [ earlier in this topic](#macie-migration-prerequisites), Macie Classic can automatically create an S3 bucket to store the data classification results that you export\. If you choose this option, Macie Classic defines the following [bucket policy](#macie-migration-results-bucket-policy) for the bucket that it creates:

```
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Sid":"MacieClassificationExportS3WriteBucketPolicy",
            "Effect":"Allow",
            "Principal":{
                "Service":"macie.amazonaws.com"
            },
            "Action":"s3:PutObject",
            "Resource":"arn:aws:s3:::{bucket_name}/*",
            "Condition":{
                "StringEquals":{
                    "s3:x-amz-acl":"bucket-owner-full-control"
                }
            }
        },
        {
            "Sid":"Deny non-HTTPS access",
            "Action":"s3:*",
            "Effect":"Deny",
            "Principal":"*",
            "Resource":[
                "arn:aws:s3:::{bucket_name}/*",
                "arn:aws:s3:::{bucket_name}"
            ],
            "Condition":{
                "Bool":{
                    "aws:SecureTransport":false
                }
            }
        },
        {
            "Sid":"Deny incorrect encryption header",
            "Action":"s3:PutObject",
            "Effect":"Deny",
            "Principal":"*",
            "Resource":"arn:aws:s3:::{bucket_name}/*",
            "Condition":{
                "StringNotEquals":{
                    "s3:x-amz-server-side-encryption-aws-kms-key-id":"{kms_key_arn}"
                }
            }
        },
        {
            "Sid":"Deny unencrypted object uploads",
            "Action":"s3:PutObject",
            "Effect":"Deny",
            "Principal":"*",
            "Resource":"arn:aws:s3:::{bucket_name}/*",
            "Condition":{
                "StringNotEquals":{
                    "s3:x-amz-server-side-encryption":"aws:kms"
                }
            }
        }
    ]
}
```