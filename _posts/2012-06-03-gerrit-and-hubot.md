---
layout: post
date: 2012-06-03
title: Gerrit and Hubot
---

For a weekend project I created a <a href="http://code.google.com/p/gerrit/">Gerrit</a>
script for <a href="http://hubot.github.com/">Hubot</a>. It lets you
query Gerrit for changes via Hubot, and enables Hubot to spam your chat
room with Gerrit events as they happen. The script is now part of the
<a href="https://github.com/github/hubot-scripts">hubot-scripts</a>
repository, so if you'd like to try it out, add <code>gerrit.coffee</code>
to your <code>hubot-scripts.json</code> file, add a <code>HUBOT_GERRIT_SSH_URL</code>
environment variable to your Hubot pointing to your Gerrit server, and
you should be good to go.

This mini-project provided a chance to learn some <a
href="http://coffeescript.org/">CoffeeScript</a>, which is actually
quite interesting for being "just" a pretty syntax on top of Javascript.

