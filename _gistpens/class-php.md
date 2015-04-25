---
ID: 3269
post_title: class.php
author: James DiGioia
post_date: 2015-02-14 18:35:26
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/default-classes-for-wp-gistpen/class-php/
published: true
---
<?php
namespace WP_Gistpen;

/**
 * This is the class description.
 *
 * @package    WP_Gistpen
 * @author     James DiGioia <jamesorodig@gmail.com> * @link http://jamesdigioia.com/wp-gistpen/ * @since [current version] */ class Boilerplate { /** * The ID of this plugin. * * @since [current version] * @access private * @var string $plugin_name The ID of this plugin. */ private $plugin_name; /** * The version of this plugin. * * @since [current version] * @access private * @var string $version The current version of this plugin. */ private $version; /** * Initialize the class and set its properties. * * @since [current version] * @var string $plugin_name The name of this plugin. * @var string $version The version of this plugin. */ public function __construct( $plugin_name, $version ) { $this->plugin_name = $plugin_name; $this->version = $version; } }