---
ID: 3157
post_title: register-ajax.php
author: James DiGioia
post_date: 2015-02-14 18:37:01
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/register-ajax-form-return/register-ajax-php/
published: true
---
add_action( 'wp_ajax_plugin_slug_insert_dialog', 'plugin_slug_insert_gistpen_dialog' ); function plugin_slug_insert_gistpen_dialog() { die(include 'path/to/dialog/form.php'); }