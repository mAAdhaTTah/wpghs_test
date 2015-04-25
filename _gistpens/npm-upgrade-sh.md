---
ID: 4184
post_title: npm-upgrade.sh
author: James DiGioia
post_date: 2015-02-15 20:10:41
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/upgrade-npm-to-latestbleeding/npm-upgrade-sh/
published: true
---
#!/bin/sh set -e set -x for package in $(npm -g outdated --parseable --depth=0 | cut -d: -f2) do npm -g install "$package" done