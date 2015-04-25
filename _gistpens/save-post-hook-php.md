---
ID: 3161
post_title: save-post-hook.php
author: James DiGioia
post_date: 2015-02-14 18:36:14
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/save_post-error-reporting-boilerplate/save-post-hook-php/
published: true
---
add_action( 'save_post', 'my_save_post_function' ); function my_save_function() {     $errors = array();     if($something_went_wrong) {         // Up to you to decide what the error message is and how you get it         $errors[] = $error_message;         // Or, if you're using WP_Error classes         $errors[] = $wp_error->get_error_message();     } }