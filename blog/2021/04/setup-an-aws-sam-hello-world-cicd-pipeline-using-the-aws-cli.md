# Setup an AWS SAM Hello, World! CI/CD Pipeline using the AWS CLI

**Author:** [Alan Mills]
**Date:** [6 April 2021 16:36]
**Tags:** [AWS, SAM, Lambda, CloudFormation, CodeCommit, CodePipeline, CodeBuild]
**Status**: Draft

This article takes you through the steps of setting up an AWS Serverless CI/CD Pipeline.

## Steps

1. [Pre-requistites](#Pre-requisities)
2. [Create Hello, World! SAM application](#Create-Hello,-World!-SAM-application)
3. [Set-up-CI/CD-Pipeline-with-CloudFormation](#Set-up-CI/CD-Pipeline-with-CloudFormation)
4. Push Hello, World! SAM application to CodeCommit
5. Verify Hello, World! SAM application deployed
6. Tearing everything down
7. Next steps

### Pre-requisities

AWS account
AWS CLI
AWS SAM
Node.js 14.x
Docker

### Create Hello, World! SAM application

Create the Hello, World! SAM application using the SAM CLI.

```bash
sam init
Template Source: 1 - AWS Quick Start Templates
Package Type: 1 - Zip (artifact is a zip uploaded to S3)
Runtime: 1 - nodejs14.x
Project name: hello-world-sam-app
AWS quick start application template: 1 - Hello World Example
```

Build and invoke SAM application to ensure everything is working

```bash
sam build
sam local invoke
```

```bash
Invoking app.lambdaHandler (nodejs14.x)
Image was not found.
Building image............................................
Skip pulling image and use local one: amazon/aws-sam-cli-emulation-image-nodejs14.x:rapid-1.21.1.

Mounting /Users/alan.mills/code/github/cadamus/AWS/hello-world-sam-app/.aws-sam/build/HelloWorldFunction as /var/task:ro,delegated inside runtime container
START RequestId: e481b48f-910a-4562-be50-b4fba0aec9dd Version: $LATEST
END RequestId: e481b48f-910a-4562-be50-b4fba0aec9dd
REPORT RequestId: e481b48f-910a-4562-be50-b4fba0aec9dd  Init Duration: 0.32 ms  Duration: 130.22 ms     Billed Duration: 200 ms Memory Size: 128 MB       Max Memory Used: 128 MB
{"statusCode":200,"body":"{\"message\":\"hello world\"}"}%
```

### Set-up CI/CD Pipeline with CloudFormation

#### Set-up files

1. Create a folder to store the CI/CD Pipeline `mkdir pipeline`
2. create the files to set-up and teardown the CI/CD Pipeline
   1. `touch setup.sh`,
   2. `touch hello-world-sam-app-pipeline.yaml` and
   3. `touch teardown.sh`
3. enable the shell scripts to execute
   1. `chmod +x setup.sh`
   2. `chmod +x teardown.sh`

#### setup.sh


## References

* [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
* [AWS Lambda now supports Node.js 14](https://aws.amazon.com/about-aws/whats-new/2021/02/aws-lambda-now-supports-node-js-14/)
* [Amazon ECR Public Gallery - Lambda - Node.js](https://gallery.ecr.aws/lambda/nodejs)
* [Docker images provided by CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-available.html)