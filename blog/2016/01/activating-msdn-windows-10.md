Activating an MSDN version of Windows 10 Enterprise
===================================================
Author: Alan Mills
Date: 31 January 2016 22:32
Tags: Windows 10

I have been running a copy of Windows 10 that I downloaded from MSDN for a while and while running in Coherence mode, the overlay about having an un-activated version of Windows 10 was getting annoying.

However, when I went used the Activation button in the Settings application, I would get the error code: 0x8007232B - DNS name does not exist.

The solution to this problem is to:
**Get the Windows 10 Enterprise Product Key**
1. Go to the [MSDN subscription website, My Product Keys](https://msdn.microsoft.com/en-us/subscriptions/keys/)
2. Search for the Windows 10 Enterprise product keys
3. If you have not claimed a key, press the **Get a Key** button
4. Copy (ctrl+c) the 20 character product key
**Activate Windows 10 Enterprise with the product key**
5. Open the Command Prompt in Administrator mode
6. Run the command SLUI 3
7. An overlay will be presented, paste (ctrl+v) the product key

Windows 10 is now activated.
