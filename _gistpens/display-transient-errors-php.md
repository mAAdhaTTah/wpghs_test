---
ID: 3167
post_title: display-transient-errors.php
author: James DiGioia
post_date: 2015-02-14 18:35:52
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-transients-to-display-errors/display-transient-errors-php/
published: true
---
if ( $errors = get_transient( "my_save_post_errors_{$post_id}\_{$user\_id}" ) ) {     foreach( $errors as $error ) { ?>         <div class="error">
  <p>
                <?php echo $error; ?>        
  </p>
</div>

<?php     }     delete_transient( "my_save_post_errors_{$post_id}" ) }