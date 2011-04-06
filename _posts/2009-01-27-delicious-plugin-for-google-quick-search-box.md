---
layout: post
tumblr_id: 73383273
date: 2009-01-27 02:51:00 UTC
title: Delicious plugin for Google Quick Search Box
---

> Update 6-12-2009: The latest version of this plugin for the latest QSB version is [here](/2009/05/29/qsb-plugins-updated-for-maganese.html).


So, a week or two ago Google released a new project - [Quick Search
Box](http://code.google.com/p/qsb-mac/). Seems to be the new Quicksilver. The
early builds seemed nice, if a bit crashy. I am by
no means a Quicksilver power user, but I did use the Delicious plugin for quick
access to my bookmarks.

Quick Search Box doesn't support Delicious (yet), so I thought I could kill a
hand-full of birds with one stone:

1. Learn some Objective-C.
2. Finally make some sort of 'open-source' contribution.
3. Make something useful for myself.

All of that to say - I've written a plugin for Quick Search Box to add
Delicious support. The code [is on
Github](http://github.com/nparry/google-quicksearchbox-plugins/tree/master).
I'm still an Xcode newbie and could not figure out how to create a working
Xcode project separate from the main QSB distribution, so you are kind of on
your own for compiling it at this point. Or, just [grab the prebuilt
plugin](http://nparry.com/software/google-quicksearchbox-plugins/delicious/Google-QSB-Delicious-v0.1.zip).

Installation instructions are included in the zip.

A few caveats:

1. At this time QSB doesn't support 'accounts' other than Google accounts, so
this plugin currently needs a hackish "put your delicious user info in a
separate file and copy it to the right spot" workaround.
2. The code isn't very robust at this point. It won't do well, for example, if
Delicious returns unexpected data in a response.

But, it works. Woohoo.

