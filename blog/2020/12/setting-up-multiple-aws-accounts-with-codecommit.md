# Setting up multiple AWS CodeCommit accounts using SSH

**Author:** [Alan Mills]
**Date:** [9 January 2021 20:22]
**Tags:** [Git, AWS CodeCommit, SSH]
**Status**: Draft


[Setting up multiple GitHub accounts](./setting-up-multiple-github-accounts.md)
  

## SSH Config

~/.ssh/config



## AWS 

```bash
ssh-keygen -t rsa -C "alan@company1.com" -f ~/.ssh/id_codecommit_company1_rsa 
aws iam upload-ssh-public-key --user-name 'alan' --ssh-public-key-body file://~/.ssh/id_codecommit_company1_rsa.pub
```

## Clone with local config

```bash
git clone ssh://codecommit_company1_ireland/v1/repos/RepoName
```

If this is the first time you have cloned a Ireland (EU-WEST-1) AWS CodeCommit repo, you will be asked to validate the RSA key fingerprint **SHA256::tKjRkOL8dmJyTmSbeSdN1S8F/f0iql3RlvqgTOP1UyQ**.  If this matches, enter **yes**.

## References

* [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html)
* [Setup steps for HTTPS connections to AWS CodeCommit repositories on Linux, macOS, or Unix with the AWS CLI credential helper](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-https-unixes.html)
* [Setup steps for SSH connections to AWS CodeCommit repositories on Linux, macOS, or Unix](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html)
* [Server fingerprints for CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/regions.html#regions-fingerprints)
