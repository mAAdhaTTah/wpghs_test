---
ID: 3164
post_title: save-errors-session.php
author: James DiGioia
post_date: 2015-02-14 18:36:06
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-_session-to-display-errors/save-errors-session-php/
published: true
---
if ( ! empty( $errors ) ) {     $\_SESSION['my\_plugin_errors'] = $errors; }