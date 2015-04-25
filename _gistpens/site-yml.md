---
ID: 4235
post_title: site.yml
author: James DiGioia
post_date: 2015-03-07 16:34:49
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/ngrok-ansible/site-yml/
published: true
---
- { role: ngrok, tags: [ngrok], when: "'development' in group_names" }