---
ID: 3147
post_title: save-transient.php
author: James DiGioia
post_date: 2015-02-14 18:35:52
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-transients-to-display-errors/save-transient-php/
published: true
---
if ( ! empty( $errors ) ) {     set_transient( "my_save_post_errors_{$post_id}\_{$user\_id}", $errors, 45 ); }