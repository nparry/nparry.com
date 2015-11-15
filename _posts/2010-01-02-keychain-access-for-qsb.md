---
layout: post
tumblr_id: 312193048
date: 2010-01-02 03:39:00 UTC
title: Keychain access for QSB
---

I recently heard that some folks get annoying password prompts every x
minutes when using my [QSB Delicious plugin](https://nparry.com/qsb_delicious_plugin/). They look like...

![Keychain prompt](/images/qsb_keychain/keychain_prompt.png)

In fact, there are at least
[two](https://code.google.com/p/qsb-mac/issues/detail?id=158&can=1&q=password&colspec=ID%20Type%20Status%20Priority%20Milestone%20Owner%20Summary%20Stars)
[separate](https://code.google.com/p/qsb-mac/issues/detail?id=548&can=1&q=password&colspec=ID%20Type%20Status%20Priority%20Milestone%20Owner%20Summary%20Stars)
issues filed for QSB about this. This is due to the access settings for the
[Mac OS X Keychain](https://en.wikipedia.org/wiki/Keychain_(Mac_OS)). In short,
some people have settings that say "Once an application is granted access it
should always get access" while others say "Force an application to
re-authorize after X minutes of idle time".

There are a couple of ways to solve this:

* Set the default keychain to never lock. This way you should only see the above prompt at most once.
* If you *want* your default keychain to lock after X minutes, you can [set up a separate keychain](http://osxfaq.com/DailyTips/09-2004/09-06.ws) just for the QSB password. This second keychain can have less security and use the never-lock setup. The keychain item you need to move to the new keychain will be called com.google.qsb.delicious.account.your_id.

To change the settings for a keychain:

* Start the Keychain Access application.
* Locate the keychain you want to change (the default keychain is usually called 'login') - all of the keychains are listed in the upper-left of the application.
* Right click on the keychain and select "Change settings for Keychain".
* You should get a dialog as shown below - you can uncheck the various "Lock when/after" settings based on what you want.

![Settings for a keychain](/images/qsb_keychain/keychain_settings.png)

The same process applies if you set up different keychains for different passwords.

I appreciate that someone took the time to email me about this, I wasn't
aware of it until they did so. If you have any questions don't hesitate to
[drop me a line](mailto:nparry@gmail.com).

