# Storing and passing variables to AWS CLI command --query

**Author:** [Alan Mills]
**Date:** [29 December 2020 01:35]
**Tags:** [AWS, CLI, Bash]
**Status**: Draft

When working with the AWS CLI there are times that you want to pass a variable to a --query and store the result of the command.

```bash
policyName='MyPolicy'
policyArn=$(aws iam list-policies --query "Policies[?PolicyName == \`$policyName\`].Arn" --output text)
echo $policyArn
```
