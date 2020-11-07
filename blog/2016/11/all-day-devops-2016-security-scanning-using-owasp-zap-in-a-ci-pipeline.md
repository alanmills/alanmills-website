All Day DevOps 2016 Security scanning using owasp zap in a ci pipeline
===========================================================================
**Author:** Alan Mills  
**Date:** [15 November 12:40](/blog/history/2016-11.md)  
**Tags:** [OWASP Zap](/blog/categories/owasp-zap.md)
**Status**: Draft

Simon Bennetts

pluggable

Point and shoot - userful but limitted
Proxying via ZAP
Manual PEN testing
Automated security regression testing
Can be used as a Debugger to see what is happening and can change traffice on the fly

Can be installed on Window, Mac OS, Docker, Linux and distros like Kali
Docker is released regularly
owas/zap2docker-stable
owas/zap2docker-weekly

``` bash
docker pull owasp/zap2docker-weekly
docker run owasp/zap2docker-weekly -t zap-baseline.py -t https://www.example.com
```

Baseline scan
* Safe, does not attack
* All release and beta passive scan rules
* Can export to XML, MarkDown, HTML and others
* Get a summary at the end
* When you scan you can use the -g command line option to generate a conf file
* Then you can use the -c command line option to re-run using the config file that you just created.

Full scan
* don't know session dropped off

There is a new Jenkins plugin about to release, expec this to be available next week (w/c )
Support Jira integration
Work best with weekly release until Zap 2.6.0 is released
Spidering, passive and active scanning
Support authentircation
part of zaproxy/community-scripts/api/sdlc-integration

* Turn off db recovery (speeds things up) `-config database.recoverylog=false`
* Update all add-ons `-addonupdate` - all scan rules are defined in add-ons, this means even if the code has not changed, you can schedule a weekly scan/build to see if new/updated rules find anything.
* Install a non default add-on `-addoninstall addonname`
* Setting the API key `-config api.key=asdfasdfasdfa`
* Missed the last one...

Using the Zap API
* RESTish, ony uses GET requests `http(s)://zap/<format>/<component>/...`
* There is a web view onto the Zap API UI
* Exporer with the Desktop UI
* Export configs from the UI (context, scan policies)
* Then reporduce using the API UI
* Finally convert to a script

Proxy regression / unit tests - effective tests will give you a through Zap scan of your application.

Kick off the spider it will give you an IP that can be used through the UI to get a view on the progress.


For complex SSO options, it maybe a good idea to have a simpler scenario in Dev/Test; this is what they do at Mozilla.
