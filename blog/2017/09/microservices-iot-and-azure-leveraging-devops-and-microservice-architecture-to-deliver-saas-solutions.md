# Microservices, IoT, and Azure: Leveraging DevOps and Microservices Architecture to Deliver SaaS Solutions 
**Author:** [Alan Mills]
**Date:** [27 September 2017 12:25]
**Tags:** [Microservices], [IoT], [Azure], [DevOps], [SaaS]
**Status**: Draft

Apress book [Microservices, IoT, and Azure: Leveraging DevOps and Microservices Architecture to Deliver SaaS Solutions]() by [Bob Familiar]() 

Table of contents:
1. From Monolithic to Microservice
2. What Is a Microservice
3. Microserive Architecture
4. Azure, A Microservice Platform
5. Automation
6. Microservice Reference Implementation
7. IoT and Microservice
8. Service Fabric

## From Monolithic to Microservice

**Lean Engineering** - Build-Measure-Lean cycle
* Build - Continuous Delivery
  * Minimum Viable Product
  * Continuous Integration
  * Automation
* Measure - Continuous Analytics
  * Real-time Monitoring - Dashboards (in-house and purchased)
  * Instrumentation
* Learn - Contiunous Feedback
  * Split Testing
  * Surveys & Interviews

DevOps - small cross-functional teams that comprise developers, architects, quality assurance and operations.  This is one of the biggest challenges to organisations.

## What Is a Microservice
This chapter covers the same material as presented in [Sam Newman]() [O'Reilly Media]() video course [The Principles of Microservices](../08/the-principles-of-microservices.md).  I would recommend watching the video course as it is more comprehensive than this chapter.

### Testing
We still need to perform testing as we are doing object-oriented development of service-oriented components.  We need to test the microservice as it moves through the deployment pipeline:
* **Internals testing** test the internals of the service including the use of data access, casching, and other cross-cutting concerns
* **Service testing** test the service implementation of the API.  This is a privmate internal implementation of the API and its associated models
* **Protocol testing** test the service at the protocol layer, calling the API over the specific wire protocol, usual HTTPS
* **Composition Testing** Test the service in collaboration with other services within the context of the solution
* **Scalabiliyt/Throughput testing** test the scalabiliyt and elasticity of the deployed microservice
* **Failover/Fault Tolerant testing** test the ability of the microservie to recover after a failure
* **PEN testing** work with a third-party software security firm to perform penetration testing.  Not this requires cooperation with your cloud provider (Azure, AWS, etc).

Careful planning, discipline, and a team approach to testing will make testing of microservices run smoothly.

## Micoservice Architecture

