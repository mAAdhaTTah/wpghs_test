---
ID: 3166
post_title: display-session-errors.php
author: James DiGioia
post_date: 2015-02-14 18:36:06
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-_session-to-display-errors/display-session-errors-php/
published: true
---
if ( array_key_exists( 'my_plugin_errors', $\_SESSION ) ) {     foreach( $\_SESSION['my_plugin_errors'] as $error ) {?>         <div class="error">
  <p>
                <?php echo $error; ?>        
  </p>
</div>

<?php     }     unset( $\_SESSION['my\_plugin_errors'] ); }