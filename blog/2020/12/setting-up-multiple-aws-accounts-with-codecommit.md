# Setting up multiple aws accounts with codecommit

**Author:** [Alan Mills]
**Date:** [18 December 2020 10:41]
**Tags:** [Git, AWS]
**Status**: Draft


[Setting up multiple git accounts](./setting-up-multiple-git-accounts.md)
  

## SSH Config

~/.ssh/config

## Clone with local config

git clone --config user.name="Alan Mills" --config user.email="alan@company1.com" --config credential.helper='!aws --profile projectName codecommit credential-helper $@' --config credential.UseHttpPath=true https://git-commit.eu-west-2.amazonaws.com/v1/repos/projectName

## AWS 


## References

* [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html)
* [Setup steps for HTTPS connections to AWS CodeCommit repositories on Linux, macOS, or Unix with the AWS CLI credential helper](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-https-unixes.html)

