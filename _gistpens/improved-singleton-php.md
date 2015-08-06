---
ID: 4503
post_title: improved-singleton.php
author: James DiGioia
post_date: 2015-08-06 00:17:02
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/a-better-wordpress-singleton/improved-singleton-php/
published: true
---
class PluginClass { protected static $instance = null; public function __construct($file) { if (static::$instance !== null) { throw new Exception; } static::$instance = $this; } public static function get() { return static::$instance; } }