---
ID: 4174
post_title: plugin-tag.sh
author: James DiGioia
post_date: 2015-02-15 20:10:40
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/git-to-svn-plugin-deploy-by-scribu/plugin-tag-sh/
published: true
---
#!/bin/bash if [ $# -lt 1 ]; then echo 'usage: plugin-tag 1.2.3' exit fi TAG_NAME=$1 git tag $TAG_NAME git push git push --tags plugin-deploy "tagging version $TAG_NAME" tags/$TAG_NAME