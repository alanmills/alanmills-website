Review of Pluralsight course: Adopting AWS: Watch This First
============================================================
**Author:** Alan Mills  
**Date:** 14 December 2016 19:07
**Tags:** Pluralsight, AWS
**Status**: Draft

The Pluralsight course [Adopting AWS: Watch This First](https://app.pluralsight.com/library/courses/adopting-aws-watch-this-first/table-of-contents), published on 17 August 2016 is given by [Lynn Monson](http://www.lmonson.com).


Overview
--------
* Course Overview
* Using Multiple Accounts
* Securing Your Accounts
* Managing Your Bill
* Choose a First Project
* Keeping a Map
* Thinking Like a Cloud

Using Multiple Accounts
-----------------------
You setup your initial AWS account and you get root credentials.  At this point you will start to build a solution using EC2, S3 and other services.

Then you look at automating the solution using Elastic Beanstalk.

Then you start to create developer accounts to work with you on AWS.  Everyone starts to do what they want to do and then you end up with the bill!  At this point you realise that you don't know what is going on, what's in development, whats in production, etc.

So a way to deal with this is to have a number of accounts with some rules:
* How are accounts created and related
* How accounts are secured

You can have different accounts around divisions in your organisations, some examples are:
* Production / development
* Business units
* Security isolation and compliance
* Failure boundaries
* Central auditing and monitoring
* and others

You can setup relationships between accounts.  This can be done to:
* Have a central paying account
* Have a one-way security relationship so one account can write its log to another account.

**start simple** only do what you need to do now, add more later.  Start with two accounts.

Securing Your Accounts
----------------------
Follow the best practices that have been found by other peoples experiments.
* Signup and setup - use a group email address
