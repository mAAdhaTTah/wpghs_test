---
ID: 3159
post_title: register-tiny-mce-plugin.php
author: James DiGioia
post_date: 2015-02-14 18:37:14
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/register-tinymce-plugin-and-button/register-tiny-mce-plugin-php/
published: true
---
add_filter( 'mce_external_plugins', 'plugin_slug_add_button' ); function plugin_slug_add_button( $plugins ) { $plugins['plugin_slug'] = 'path/to/editor/plugin.js'; return $plugins; } add_filter( 'mce_buttons', 'plugin_slug_register_button' ); function plugin_slug_register_button( $buttons ) { array_push( $buttons, 'plugin_slug' ); return $buttons; }