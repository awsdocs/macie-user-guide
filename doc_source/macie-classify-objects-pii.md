--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Personally Identifiable Information<a name="macie-classify-objects-pii"></a>

Object classification by personally identifiable information \(PII\) is based on recognizing any personally identifiable artifacts based on industry standards such as NIST\-80\-122 and FIPS 199\. Macie Classic can recognize the following PII artifacts: 
+ Full names
+ Mailing addresses
+ Email addresses
+ Credit card numbers
+ IP addresses \(IPv4 and IPv6\)
+ Drivers license IDs \(USA\)
+ National identification numbers \(USA\)
+ Birth dates

As part of PII object classification, Macie Classic also assigns each matching object a PII impact of high, moderate, and low using the following criteria:
+ High
  + >= 1 full name and credit card
  + >= 50 names or emails and any combination of other PII
+ Moderate
  + >= 5 names or emails and any combination of other PII
+ Low
  + 1–5 names or emails and any combination of PII
  + Any quantity of PII attributes above \(without names or emails\)