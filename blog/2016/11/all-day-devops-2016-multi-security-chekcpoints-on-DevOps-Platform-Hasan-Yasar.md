all-day-devops-2016-multi-security-chekcpoints-on-DevOps-Platform-Hasan-Yasar
===========================================================================
**Author:** Alan Mills  
**Date:** [9 November 13:20](/blog/history/2016-11.md)  
**Tags:** [SDLC](/blog/categories/owasp-zap.md), [DevOps](/blog/categories/devops.md)
**Status**: Draft


Multiple Dimensions of DevOps
* Culture
* Process and Practices
* Automation and Measurement
* System and Architecture

Multi Security Checkpoint DevOps Security Platform
* CI
* Application code
* Infrastructure
* Documentation - has to be part of the source code otherwise developers will not update the documentation
* Monitoring
* Communication System
* Issue tracking
* Code Review

People
------
* People need to understand the security too, amongst all the other demands
* Security is another story for the team
* Security has to be part of the development team not a blocker, they need to share in the work.


By 2020 80% of software will be using cloud services.


Typical development lifecycle, that then is extended for product management and operations.
Where to add security
1. In the requirements
2. Project configuration - secured and hardened environments given to developers from the beginning
3. Code review - code scanning for security e.g. SQL injection, proper initalisation, get a security expert to review code,
4. Continuous Integration & Testing - static scanning
5. QA / Integration Testing - Zap, Nessus, etc.
6. Transition - full blown security testing and compliance checks

Security must not break the rapid feedback to all stakeholders in the development team and delivery of value to customers.  Security experts needs to be able to give the security issue feedback to the developer and where needed explain the implication of the security issue and how to fix the issue.

Not everything can be automated.  Manual review must not block the CD e.g.
* Static analysis
* Peer code review
* PEN testing

The human interaction should happen between CI and CD.  Then there is a proposal to have a hold for security based on a security schedule.  This looks like a DevOps anti-pattern.
