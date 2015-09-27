---
ID: 4493
post_title: normal-singleton-and-boot.php
author: James DiGioia
post_date: 2015-09-27 01:07:59
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.ngrok.com/gistpens/a-better-wordpress-singleton/normal-singleton-and-boot-php/
published: true
---
class PluginClass { public static $instance = null; public static function init() { if ( null === self::$instance ) { self::$instance = new PluginClass(); self::$instance->boot(); } return self::$instance; } protected function __construct() { // Startup } protected function boot() { // Boot } } PluginClass::init();