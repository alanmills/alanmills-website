Review of Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation, Video Enhanced Edition (2010/07)
==========================================================================================================================================
**Author:** Alan Mills  
**Date:** [25 November 13:30](/blog/history/2016-11.md)  
**Tags:** [Continuous Delivery](/blog/categories/data-driven-design.md)
**Status**: Draft

I have skimmed over and made reference to the seminal book [Continuous Delivery](http://amzn.to/2fyFeX9) by [Jez Humble](https://jezhumble.net) and [David Farley](http://www.davefarley.net) for years.  As I am in the process of setting up a new program of work where we are going to be using contiunous delivery from day 1, I thought that it was about time that I read this important book rather than working off presentations, podcasts and articles by the authors and others in the industry.

What I love about this book is that they focus on the critical aspects of a project outside of software development.  The book focuses on:
* Build
* Deployment
* Test
* Release

The deployment pipeline
* Commit stage
  * Compile
  * Unit test
  * Analysis
  * Build installers
* Automated acceptance testing
* Automated capacity testing
* Manual testing
  * Showcases
  * Exploratory testing
* Release


The first chapter sets the scene for why you should do continuous delivery, initially I was thinking of skipping over this as I'm already convinced that continuous delivery to production is the only way to work, re-reading this section made my blood boil as Jez and Dave so clearly articulate all of the pain associated with manual deployments.

> **release software** as software professionals love to delivery high-quality, valuable software in an efficient, fast, and reliable manner.  - it is only useful when **customers** make use of the software, therefore, we have to **measure** usage.
> - Jez Humble & David Farley, Continuous Delivery

**reduce cycle time** you can only get value from software once it is released to customers.

**resource optimisation** free up people to do high value activities, leave the machine to do boring, repetitive activities e.g. deployment and regression testing.

Tests in the commit stage
-------------------------
* fast
* >75% coverage of the codebase
* If a test fails it equals a critical error and the solution cannot be released.  Therefore, tests for the colour of a UI element should not be included.
* As environment -netural e.g. not a replication of production - so performance tests are not in this stage.

Tests in other stages
---------------------
* These tests are only run if the commit stage tests pass
* Slower to run
* Need to be parallisable
* We may wish to over-ride a failed test e.g. a critical fix may cause the performance to drop below a threshold and therefore, we will release anyway.
* They run in an environment as similar to production as possible
* Therefore, as well as testing the focus of the tests, they also test the deployment process and any changes to the production environment

Deploy which version you want
-----------------------------
* Testers can run different versions of the software to validate changes of behavior in newer versions
* Support can deploy a released version of the application into a support environment to validate a defect
* Operations can select a known good build to deploy to production as part of a disaster recovery exercise
* Releases can be performed at the push of a button

* Every developer should have their own environment using the same deployment processes however, they may need to be able to build code and other special steps.  Even here we should keep the deployment process as similar to the other environments as possible.


> **Testing is not a stage** and **testing is not the domain, purely or even principally, of testers**
> - Jez Humble & David Farley, Continuous Delivery

**Done** means that the software **is in the hands of the customer**.

**Deming cycle:** plan, do, study, act

Configuration Management
------------------------
**Often overlooked** Configuration Management has a distinct impact on how your team collaborates

Can you answer yes to all of these?
* Can I exactly reproduce any of my environments, including the version of the operating system, its patch level, the network configuration, the software stack, the applications deployed into it, and their configuration?
* Can I easily make an incremental change to any of these individual items and deploy the change to any, and all, of my environments?
* Can I easily see each change that occurred to a particular environment and trace it back to see exactly what the change was, who made it, and when they made it?
* Can I satisfy all of the compliance regulations that I am subject to?
* Is it easy for every member of the team to get the information they need, and to make the changes they need to make? Or does the strategy get in the way of efficient delivery, leading to increased cycle time and reduced feedback?


Use for everything
------------------
* The usuals: code, tests, database scripts, build and deployment scripts
* Not always done: documentation, libraries and configuration files for your applications, compiltes
* Have your tools in VC
* Operating system(s), DNS zone files, firewall configuraitons, etc
* **You need everything require to re-create your applications binaries and the environments in which they run**

* Store binary images of application servers, compilers, virtual machines and other parts of your toolchain.
* This allows you to get everything for a given release from a single point. **dependencies we should do this for packages however, this can make the VC check-out very large, especially when working with Git...**

Do not store the binary output of your applications compilation, this is because:
* They are Big
* They proliferate rapidly e.g. everytime you check-in your code another binary is created, which will happen a lot over a large project with a lot of people working on it.
* You should not re-create binaries however, they are not stored in VC as this can cause problems in the **deployment pipelines**.  This is that can cause confusion with versioning if you have two check-ins for a version (the source change and the subsequent compiled binaries).

**Everyone works on main**  
* break your work down into small changes
* pull all latests changes from main
* run the commit test set
* then push to main

**Does Git have pretested commits?**
**We should be checking in at least everyday if not several times a day**

Do this after every incremental change or refactoring.  Doing this regularly will get the most benefit from continuous integration.

Commit messages
---------------
Multi-line - first line is a summary
Second line is to give the reader enough information to understand the change with a link to the project management tool for the feature or bug **How do we do this in such a way to not have issues when that tool is upgraded or changed?**

Components
----------
**Have binary dependencies between components not source dependencies**
Each component will have its own **deployment pipeline**

There can be a problem re-building across a number of components when their are many components in-play however, there are tools to help with this.

Software configuration
----------------------
If there are software configuration items in your solution, this needs to be managed like everything else with version control and tests *"smoke-test your deployments"*.  **It is a myth that configuration change is any less risky than code change**

Load configuration at startup time or run time not at build or package time.


Keep configuration information in VC but this is often separate from the source code as you need these per environment.  **Do not store passwords and other secrets in VC**, this should be entered by the person doing the deployment.  For handling authentication in multilayer systems use certificates, directory service or SSO.

**Wrap the getting of configuration information** this way you can fake it in your tests.  Model the configuration as a tuple.

Make sure that different environments cannot talk to each other to ensure that you don't think that the system is working (or broken) and it's because of incorrect configuration.

### Deployment test
1. have tests to validate that all items setup by configuration are up and running and accessible through the correct URI.
2. Have some more test to make sure that these services are running as expected based on the configuration settings.


Managing your environments
--------------------------
