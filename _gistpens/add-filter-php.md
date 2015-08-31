---
ID: 4546
post_title: add-filter.php
author: James DiGioia
post_date: 2015-08-31 10:52:42
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/use-wordpress-github-sync-with-markdown-on-save/add-filter-php/
published: true
---
add_filter('wpghs_content_export', function($content, $wpghs_post) { return $wpghs_post->post->post_content_filtered; });