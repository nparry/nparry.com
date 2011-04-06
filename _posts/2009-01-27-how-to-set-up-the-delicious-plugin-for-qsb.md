---
layout: post
tumblr_id: 73615665
date: 2009-01-27 23:55:00 UTC
title: How to set up the Delicious plugin for QSB
---

> Update: This version of the plugin has been superseded by [version 0.2](/2009/02/04/delicious-plugin-for-qsb-v0-2.html)


Right now setting up my Delicious plugin isn't the easiest thing in the
world - so here are some illustrated step-by-step instructions!

Download and unzip the plugin ([Available here](http://nparry.com/software/google-quicksearchbox-plugins/delicious/Google-QSB-Delicious-v0.1.zip))

![](/images/howto_setup_delicious_plugin/01_download_plugin.png)

Copy the plugin file to the right directory. This will be
/Users/your_user_name/Library/Application Support/Google/Quick
Search Box/PlugIns

![](/images/howto_setup_delicious_plugin/02_copy_plugin.png)

Copy the plist file to the right directory. This will
be /Users/your_user_name/Library/Application
Support/Google/Quick Search Box

![](/images/howto_setup_delicious_plugin/03_copy_plist.png)

Open the plist file with TextEdit. Put in your Delicious username
and password and save your changes.

![](/images/howto_setup_delicious_plugin/04_edit_plist.png)

Restart Google Quick Search Box and verify that the Delicious plugin
is listed. Under the S column (for 'Sources' I think) there should be a 1.

![](/images/howto_setup_delicious_plugin/05_verify_plugin.png)

Make sure that Delicious Bookmarks are selected as a searchable
item.

![](/images/howto_setup_delicious_plugin/06_verify_searchable_items.png)

If all has gone well, your Delicious bookmarks and tags should show
up as search results.

![](/images/howto_setup_delicious_plugin/07_search_by_bookmarks_and_tags.png)

You can pivot on tags to narrow the results to just those bookmarks
with the given tag.

![](/images/howto_setup_delicious_plugin/08_pivot_on_tags.png)

