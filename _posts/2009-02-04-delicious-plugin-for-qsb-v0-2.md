---
layout: post
tumblr_id: 75707712
date: 2009-02-04 23:00:00 UTC
title: Delicious plugin for QSB, v0.2
---

> Update: This version of the plugin has been superseded by [version 0.3](/2009/03/13/qsb-plugins-updated-for-vanadium.html)


Google released an [updated version of
QSB](https://groups.google.com/group/qsb-mac-discuss/browse_thread/thread/8889dbfe98a40ab4)
with support for custom account types. Woohoo! So, I've updated my Delicious
plugin to take advantage of the new feature.

The code is of course [available on
GitHub](https://github.com/nparry/google-quicksearchbox-plugins/tree/delicious_v0.2).
You can grab the pre-built plugin [right
here](https://assets.nparry.com/software/google-quicksearchbox-plugins/delicious/Google-QSB-Delicious-v0.2.zip).

Installation is a bit easier this time:

* Make sure you are running the Titanium QSB release.
* Download & unzip the plugin.
* Copy the plugin to /Users/your_user_name/Library/Application Support/Google/Quick Search Box/PlugIns
* Restart QSB.
* Open the preferences, go to Accounts and add a Delicious account.
* Once the account is added, make sure Delicious is checked under Searchable Items.

If you installed v0.1 with the custom plist file containing your
Delicious username and password you can delete that file.

