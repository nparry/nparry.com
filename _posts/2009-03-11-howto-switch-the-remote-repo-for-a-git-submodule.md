---
layout: post
tumblr_id: 85402847
date: 2009-03-11 02:56:37 UTC
title: HowTo -  Switch the remote repo for a git submodule
---

For my QSB plugins I was using a git submodule pointing to a repository
managed by someone else - I needed some updates to that module, so I
forked it and made them myself. Thus, I needed my QSB plugin project to
point to my new repo for the submodule. I did it this way:

* Edit .gitmodules to change the URL for the submodule to my repository
* git submodule sync
* cd path/to/submodule
* git remote update
* git merge origin/master
* Add .gitmodules and path/to/submodule with 'git add', then commit

I used [this page about
submodules](http://woss.name/2008/04/09/using-git-submodules-to-track-vendorrails/)
to figure some of this out.

