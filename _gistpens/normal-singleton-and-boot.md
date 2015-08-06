---
ID: 4493
post_title: normal-singleton-and-boot
author: James DiGioia
post_date: 2015-08-06 00:09:52
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/a-better-wordpress-singleton/normal-singleton-and-boot/
published: true
---
class PluginClass { public static $instance = null; public static function init() { if ( null === self::$instance ) { self::$instance = new PluginClass(); self::$instance->boot(); } return self::$instance; } protected function __construct() { // Startup } protected function boot() { // Boot } }