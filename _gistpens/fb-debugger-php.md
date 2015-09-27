---
ID: 4568
post_title: fb-debugger.php
author: James DiGioia
post_date: 2015-09-27 01:54:42
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/facebook-post-debugger/fb-debugger-php/
published: true
---
<?php /* Plugin Name: Facebook Post Debugger Version: 0.1 Plugin URI: http://jamesdigioia.com/ Description: This plugin runs the bit.ly shortlink through the Facebook debugger upon publishing. Author: James DiGioia Author URI: http://www.jamesdigioia.com/ */ add_filter( 'publish_post', 'fb_debug_link' ); function fb_debug_link( $post ) { $short = wp_get_shortlink($post['id']); $url = 'https://graph.facebook.com/?id='.$short.'&scrape=true'; wp_remote_post( $url ); }