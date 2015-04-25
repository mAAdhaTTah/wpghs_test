---
ID: 3192
post_title: 5-3-or-greater.php
author: James DiGioia
post_date: 2015-02-14 18:35:32
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/php-version-check-for-wp-boilerplate/5-3-or-greater-php/
published: true
---
public static function activate() { if ( ! version_compare( PHP_VERSION, '5.3', '<' ) ) { return; } deactivate_plugins( 'url-embedlifier' ); wp_die('
The **URL Embedlifier** plugin requires PHP version 5.3 or greater.', 'Plugin Activation Error', array( 'response' => 200, 'back_link' => true ) ); }