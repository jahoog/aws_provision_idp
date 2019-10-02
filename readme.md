# Overview
The IDPProvision.YML is a Cloudformation template that installs an IAM Identity Provider.  It includes the following resources:

- A Lambda function and corresponding IAM role used to execute the Lambda Function - this functions as a custom backed resource for CloudFormation since CloudFormation does not support the IAM IDP resource directly
- Identity Provider (delivered via a Lambda backed customer resource written in Python 3.7)
- 4 IAM Roles:
  - AdminRole - uses the managed policy ARN: arn:aws:iam::aws:policy/AdministratorAccess
  - BillingRole - uses the managed policy ARN: arn:aws:iam::aws:policy/job-function/Billing
  - DeveloperRole - uses the managed policy ARN: arn:aws:iam::aws:policy/PowerUserAccess
  - ReadonlyRole - uses the managed policy ARN: arn:aws:iam::aws:policy/ReadOnlyAccess

# Pre-requisites
You will need a SAML IDP, such as ADFS, Shibboleth, or the like.  You must provide the IDP federation metadata and correspondingly have configured your IDP to send SAML assertions to AWS.  Some helpful links to get you started:

- https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html
- https://aws.amazon.com/blogs/security/aws-federated-authentication-with-active-directory-federation-services-ad-fs/
- http://federationworkshopreinvent2016.s3-website-us-east-1.amazonaws.com/

# Implementation
This is a CloudFormation template that requires 2 parameters:

- SamlProviderName: the name of the IDP within AWS IAM - this should match your SAML assertion value(s)
- SamlProviderUrl: the SAML federation metadata - this should be accessible publicly via https://... or s3://...

You can also deploy this template using CloudFormation StackSets in conjuction with AWS Control Tower or Landing Zone accounts.  In that case, nothing changes other than the following the instructions for deploying a StackSet: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html

If you are using AWS Control Tower, you should use the following roles in the StackSet properties:

- StackSet admin role: AWSControlTowerStackSetRole
- StackSet execution role: AWSCloudFormationStackSetExecutionRole