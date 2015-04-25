---
ID: 3145
post_title: add-redirect-args.php
author: James DiGioia
post_date: 2015-02-14 18:35:44
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-redirect-args-to-display-errors/add-redirect-args-php/
published: true
---
if ( ! empty( $errors ) ) {     add_filter('redirect_post_location', function( $location ) use ( $errors ) {         return add_query_arg( 'gistpen-errors', implode( ",", $errors ), $location );     }); }