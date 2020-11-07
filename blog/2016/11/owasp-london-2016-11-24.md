OWASP London 24 November 2016
=============================
**Author:** Alan Mills  
**Date:** [24 November 18:45](/blog/history/2016-11.md)  
**Tags:** [OWASP London](/blog/categories/owasp-london.md)
**Status**: Draft

A lovely location with drinks and food provided by [Empiric UK](http://t.co/lD5lb02WPT)

Agenda
------
* Introduction and OWASP News - Sam Stepanyan and Sherif Mansour
* Lightning Talk 1 - OWASP ZAP Official Jenkins Plugin walkthrough & Demo - Goran Sarenkapa
* PCI - The View from the Bridge - Jeremy King
* JSON Hijacking - Gareth Heyes
* Lightning Talk 2 - myBBC Security Council - What It Means To You - Shane Kelly


Introduction and OWASP News - Sam Stepanyan and Sherif Mansour
--------------------------------------------------------------
### Notices and updates
* [OWASP LONDON Hackaton CTF](https://www.eventbrite.co.uk/e/owasp-london-hackathon-and-ctf-tickets-29190284928?aff=eioscale&ref=eioscale) - This is for developers, Monday is showing how the code is in the code and Tuesday is the CTF competition (with prizes!).
* [Owasp-devSecCon-Summit, England, April 2017](https://www.owasp.org/index.php/Owasp-DevSecCon-Summit)
* [OWASP VBScan Project](https://www.owasp.org/index.php/OWASP_VBScan_Project) - this is a scanner for analysing vBulletin CMS vulnerabilities
* [Mobile AppSec Verification standard](https://www.owasp.org/index.php/OWASP_Mobile_Security_Project)
* [OWASP Mobile Top 10 2016 - Release Candidate](https://www.owasp.org/index.php/Mobile_Top_10_2016-Top_10)
* [OWASP Dependency Checker](https://www.owasp.org/index.php/OWASP_Dependency_Check) - can scan your project for known vulnerabilities
* [OWASP Zed Attack Proxy Project](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)

Lightning Talk 1 - OWASP ZAP Official Jenkins Plugin walkthrough & Demo - Goran Sarenkapa
-----------------------------------------------------------------------------------------
Goran is the lead developer for the plugin.
[Official OWASP Zed Attack Proxy Jenkins Plugin](https://wiki.jenkins-ci.org/display/JENKINS/zap+plugin)
This plugin is to make your life easier if you are working with Jenkins, you can use other tools but you have to write this yourself.

OWASP Zap project have tried to document the first release to make it easier to develop the integration.

You can do pretty much everything you can do with Zap through the plugin.
An authenticated spider scan will be coming soon.

Install Jenkins, requires 1.580.1+ to run
For using Selenium, the Zap plugin has only be tested with Firefox and you set Firefox local proxy settings as you would for Zap.

**Idea** Set this up as a docker image and Vagrant VM for Firefox?  Have a look to see if anyone has done a Docker image for Selenium.

When configuring Jenkins, you have to give the session a unique context name or it will break the build.
**Idea** How do you configure Jenkins outside of the commandline? so this stuff can be stored in Git. **Goran** suggested that you can do this through Selenium.

There is a report plugin (this is through the Jenkins marketplace and Goran has not released this yet), also written by Goran.
When outputing the report you have to name it the same as the job but with the extension.

**Question** How do we get the build to break when there are alerts?

Ask questions on the [Google Group](https://groups.google.com/forum/#!forum/zaproxy-jenkins)

PCI - The View from the Bridge - Jeremy King
--------------------------------------------
[Jeremy King](https://uk.linkedin.com/in/jeremy-king-90a75328)

Build on what has come before and realise that you cannot predict everything that is going to happen. PCI follows the usual mantra of:
* People
* Process
* Technology
Oh and a bit of luck.

Often we try to do things to be *compliant* instead doing the right thing, as sometimes people just do a half-arsed job that leaves the user with no real choice.

What is the impact of Brexit = none, there is new regulation coming in 2018, we have 18 months to get this done.
* European General Data Protection regulation
* European Payment Services Directive 2
* EBA Securing Internet Payments
* European Interchange Fee

Most of the regulation is high level however, they are getting into more details when it relates to **fines and penalties**.  Personal data breach you have to tell your regulator within 72 hours of becoming aware of it.
The fines are moving from a maximum of 500M to 4% of total worldwide annual turner of the preceding financial year.

### Changes to PCI DSS 3.1
* SSL and early TLS is done, we have to use the most recent version of TLS however, this is not mandated in the PCI DDS 3.1
* Two-factor authentication has been replaced with multi-factor.  The factors have to be different e.g. two passwrds is regarded as one factor.
* Administrators have to have two-factor as too many breaches have shown that weak passwords were part of the problem.
* Trying to get rid of cardholder data through P2PE (Point-to-point encryption)
* Setting up a software security task force - this is to understand the change of software and how this needs to be included in an organisation change process.

JSON Hijacking - Gareth Heyes
-----------------------------
[Gareth Heyes](https://twitter.com/garethheyes) works for (PortSwigger)[https://portswigger.net/] how make the [Burp suite](https://portswigger.net/burp/)

History of hacking, that have now been fixed in every browser
* array litteral Attack
* Object.prototype setter attack

Jpeg attack
[Ange Albertini](http://pics.corkami.com)

Using the attack

has trap using the Javascript Proxy, this works for the defined function as well as undefined variables.

Window.__proto__ = function(__proto__) {}


use character set encoding to prevent these attacks.
Make sure that uploaded images are on another domain with CSP policies that do not allow scripts.
You can re-write the jpeg header to remove the Javascript.

Lightning Talk 2 - myBBC Security Council - What It Means To You - Shane Kelly
------------------------------------------------------------------------------

Shane Kelly

This talk was a little boring as the presentation case clearly an internal presentation, at a high-level for what the myBBC AppSec team does within the BBC.  The presentation itself could be useful to reference when creating a similar presentation within your organisation.

myBBC is the personalisation services that BBC have been adding to their sites.  They have an AppSec project where they track the status of security across the multiple projects across myBBC.  When an issue is found they create a ticket that is placed on the project backlog.
