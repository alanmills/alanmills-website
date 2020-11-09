# AWS CLI error message InvalidClientTokenId

**Author:** [Alan Mills]
**Date:** [9 November 2020 20:41]
**Tags:** [AWS, CLI]
**Status**: Draft

When using the AWS CLI interfaces (aws/sam/etc.), you can receive the error code InvalidClientTokenId.  This occures when the Client ID set up on your workstation does not match the Client ID set up on AWS.  

## Check the valid AWS Client IDs

### Configured AWS Access Keys on your Account

Login in to the AWS page [My security credentials](https://console.aws.amazon.com/iam/home?#/security_credentials)

On the **AWS IAM credentials** tab, there is a section **Access keys for CLI, SDK, & API access** where there will be a table of access keys e.g.

| Access key ID        | Status | Created              | Last used            | Actions                             |
| -------------------- | ------ | -------------------- | -------------------- | ----------------------------------- |
| GFIAX2T5CTPO9PUMGON3 | Active | 2020-07-30 16:40 UTC | 2020-11-09 20:27 UTC | [Make inactive]() &#124; [Delete]() |
| TAXAP9B2ITQM9PWEHJI7 | Active | 2020-11-09 20:31 UTC | 2020-11-09 20:35 UTC | [Make inactive]() &#124; [Delete]() |

### Configured aws cli AWS Access Key

```bash
aws configure

AWS Access Key ID [****************SIBF]:
```

## Set up a new Access Key

For more information on AWS access keys, refer to the the AWS Documentation, [Managing access keys (console)[(https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html?icmpid=docs_iam_console#Using_CreateAccessKey)

If you may need to **Delete** an Access Key before you **Create access key**.
Note, you cannot get the **Secret Access Key** after creating it, so complete the next step [Update the aws cli with your AWS Access Key and Secret Access Key]()

> Do not share your Access Key or Secret Access Key with anyone, treat it like any other username/password

## Update the aws cli with your AWS Access Key and Secret Access Key

From the `aws configure` command, paste in your Access Key and Secret Access Key

```bash
aws configure

AWS Access Key ID [****************SIBF]: GFIAX2T5CTPO9PUMGON3
AWS Secret Access Key [****************p3vq]: ABaba9871345234+gSLHSEHafd/asdf92Nparnkw 
Default region name [eu-west-1]: <return>
Default output format [json]: <return>
```
