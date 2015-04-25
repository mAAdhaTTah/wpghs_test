---
ID: 3617
post_title: write-log.php
author: James DiGioia
post_date: 2015-02-14 18:33:52
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/wordpress-error-logging/write-log-php/
published: true
---
if (!function_exists('write_log')) { function write_log ( $log ) { if ( true === WP_DEBUG ) { if ( is_array( $log ) || is_object( $log ) ) { error_log( print_r( $log, true ) ); } else { error_log( $log ); } } } }