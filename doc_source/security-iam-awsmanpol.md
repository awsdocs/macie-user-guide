--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# AWS Managed Policies for Amazon Macie and Amazon Macie Classic<a name="security-iam-awsmanpol"></a>





To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.













## Updates to AWS Managed Policies for Macie and Macie Classic<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for both Macie and Macie Classic since Macie began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Macie Classic [Document History](doc-history.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AmazonMacieServiceRolePolicy](using-service-linked-roles.md) – Update to an existing policy  |  Macie added Amazon CloudWatch Logs actions to the *AmazonMacieServiceRolePolicy* policy\. These actions allow the new Macie to publish log events to CloudWatch Logs for sensitive data discovery jobs\.  | April 13, 2021 | 
|  Macie started tracking changes\.  |  Macie started tracking changes for its AWS managed policies\.  | April 13, 2021 | 