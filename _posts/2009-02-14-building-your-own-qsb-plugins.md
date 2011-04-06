---
layout: post
tumblr_id: 78202946
date: 2009-02-14 05:20:13 UTC
title: Building your own QSB plugins
---

In building my own QSB plugins I have just been adding my code to the
main QSB xcode project. This was less than ideal, since every time a
new QSB version was released my changes to the xcode project file would
get wiped out. It was also a bit tricky since QSB lives in subversion
and I was managing my plugins with git.

Happily, I think I've improved the situation a bit.

First, many thanks to Steven Noonan who set up a [QSB mirror on
github](http://github.com/tycho/qsb-mac/tree/master) - I'm now able to include
the QSB code base in my own plugin project via git submodules.

Next, I continued my slow learning experience with xcode and eventually figured
out how to build my plugins in their own project. This is
probably obvious to experience xcoders, but my eureka moment occurred
when I realized you could drop other xcode project files into your own
project as sub-projects. This let me add the QSB Vermilion framework
from the aforementioned git submodule as a dependency of my plugins. I had a
bit of trouble getting xcode to include Vermilion headers the
right way - the [FRAMEWORK_SEARCH_PATH trick on this
page](http://www.devklog.net/2007/10/07/improving-the-xcode-sub-project-experience-with-scripts-and-wits/)
helped me through that.

So - the end result - I have an xcode project just for my plugins that
should gracefully handle updates to the main QSB code base. And I
learned a bit about git and xcode along the way ;-)

If you are interested you can check out the [xcode project in the github
repository](http://github.com/nparry/google-quicksearchbox-plugins/tree/master).

