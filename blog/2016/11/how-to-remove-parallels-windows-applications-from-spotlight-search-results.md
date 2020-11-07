How to remove Parallels Windows applications from Spotlight search results
==========================================================================
**Author:** Alan Mills  
**Date:** [8 Novemeber 2016 17:22](/blog/history/2016-11.md)  
**Tags:** [Parallels](/blog/categories/parallels.md), [macOS 10.12 Sierra](osx-10-12-sierra), [Windows 10](/blog/categories/windows-10.md)
**Status**: Publish

I have an number of applications installed on both my Mac and on one or more of my Parallels Windows virtual machines.  I also like to launch applications on macOS using [Spotlight](https://support.apple.com/en-us/HT204014).  

Unfortunately sometimes I accidentally launching the Windows version of an application when I want the Mac one.  This then causes Parallels to start-up the relevant Windows VM, which can cause all sorts of issues when done at the wrong time.

To stop this happening you have a number of options
* [Be extra careful to not run the  Windows application from the Spotlight results](be-careful-and-not-run-the-windows-application)
* [Disable Windows applications from sharing with Mac](disable-windows-applications-from-sharing-with-mac)
* [Remove Parallels applications from Spotlight](remove-parallels-applications-from-spotlight)

Be careful and not run the Windows application
----------------------------------------------
Lets be honest, you're not reading this blog post if you're already able to do this 100% of the time.

Disable Windows applications from sharing with Mac
--------------------------------------------------
This can be achieved by telling Parallels to not [Share Windows applications with Mac](http://download.parallels.com/desktop/v7/ga/documentation/en_US/Parallels%20Desktop%20User's%20Guide/33332.htm).

If you do this you will no longer be able to open macOS files with Windows applications.

Therefore if you want to keep this capability then you will need to follow the steps in the next section ([Remove Parallels applications from Spotlight](remove-parallels-applications-from-spotlight)).


Remove Parallels applications from Spotlight
--------------------------------------------
To prevent Spotlight from showing you Windows applications, use the Spotlight Privacy settings.  When set correctly this tells Spotlight to stop indexing the Parallels applications folder in your macOS home directory.

1. Open **System Preferences**
2. Select **Spotlight**
3. Select **Privacy**
4. **Add** the path to the Parallels applications `~/Applications (Parallels)`
