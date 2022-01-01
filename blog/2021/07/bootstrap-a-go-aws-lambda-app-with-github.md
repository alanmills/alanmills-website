# Bootstrap a CI/CD Serverless Go app using AWS and Github 

**Author:** [Alan Mills]
**Date:** [10 July 2021 10:56]
**Tags:** [Go, AWS Lambda, AWS CDK]
**Status**: Draft

## Overview

When starting a new project there are a number of things that need to be put in-place to ensure a team is successful, these are:
* Version Control
* Code Pipelines
* Automated Testing

Simple case

* Monorepo - single repository for all assets
* Single deployment - deploy everything together


## Release Pipeline

* <5 minutes
  * Commit
  * Build
  * Unit Testing
* <1 hour
  * Static Analysis
  * Acceptance Testing
  * Security Testing
  * Performance Testing
  * Scalability Testing
  * Sign-Offs
  * Deploy

## References

* [YouTube - How to Build a DEPLOYMENT PIPELINE? (Continuous Delivery)](https://youtu.be/x9l6yw1PFbs)
* [AWS Docs - Building Lambda functions with Go](https://docs.aws.amazon.com/lambda/latest/dg/lambda-golang.html)
* [AWS Docs - Working with the AWS CDK in Go](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-go.html)
