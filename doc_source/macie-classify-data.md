--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Classifying Data with Amazon Macie Classic<a name="macie-classify-data"></a>

Macie Classic can help you classify your sensitive and business\-critical data stored in the AWS cloud\. Currently, Macie Classic analyzes and processes data stored in Amazon S3 buckets\. To classify your data, Macie Classic also uses the ability in AWS CloudTrail to capture object\-level API activity on S3 objects \(data events\)\. However, Macie Classic monitors CloudTrail data events only if you specify at least one S3 bucket for Macie Classic to monitor\. 

After you specify the S3 bucket or buckets for Macie Classic to monitor, you enable Macie Classic to continuously monitor and discover new data as it enters your AWS infrastructure\. For more information, see [Specifying Data for Macie Classic to Monitor](macie-integration.md#macie-integration-services)\.

**Limits**
+ Macie Classic has a default limit on the amount of data that it can classify in an account\. After this data limit is reached, Macie Classic stops classifying the data in this account\. The default data classification limit is 3 TB\. You can contact AWS Support and request an increase to the default limit\.
+ If you specify S3 buckets that include files of a format that isn't supported in Macie Classic, Macie Classic doesn't classify them\.
+ Macie Classic's content classification engine processes up to the first 20 MB of an S3 object\.

Your Macie Classic usage charges include only the costs for the content that Macie Classic processes\. For example, Macie Classic can't extract text from \.wav files \(images or movies\); therefore, it doesn’t process that content and you’re not charged for it\.

**Topics**
+ [Supported Compression and Archive File Formats](macie-compression-archive-formats.md)
+ [Content Type](macie-classify-objects-content-type.md)
+ [File Extension](macie-classify-objects-file-extension.md)
+ [Theme](macie-classify-objects-theme.md)
+ [Regex](macie-classify-objects-regex.md)
+ [Personally Identifiable Information](macie-classify-objects-pii.md)
+ [Support Vector Machine–Based Classifier](macie-classify-objects-classifier.md)
+ [Object Risk Level](#compound-score)
+ [Retention Duration for S3 Metadata](#metadata-retention-duration)

## Object Risk Level<a name="compound-score"></a>

Through the automatic classification methods previously described, an object that Macie Classic monitors is assigned various risk levels based on each content type, file extension, theme, regex, and SVM artifact that is assigned to it\. The object's compound \(final\) risk level is then set to the highest value of its assigned risk levels\.

## Retention Duration for S3 Metadata<a name="metadata-retention-duration"></a>

Macie Classic stores metadata about your S3 objects for the default duration of 1 month\. You can extend this duration up to 12 months\.