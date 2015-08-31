---
ID: 4545
post_title: add-filter
author: James DiGioia
post_date: 2015-08-31 10:52:41
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/use-wordpress-github-sync-with-markdown-on-save/add-filter/
published: true
---
add_filter('wpghs_content_export', function($content, $wpghs_post) { return $wpghs_post->post->post_content_filtered; })