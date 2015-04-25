---
ID: 3169
post_title: display-get-errors.php
author: James DiGioia
post_date: 2015-02-14 18:35:44
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/using-redirect-args-to-display-errors/display-get-errors-php/
published: true
---
if ( array_key_exists( 'gistpen-errors', $_GET) ) { ?>     <div class="error">
          <p>
    <?php
            echo 'The post saved with error codes: ' . $_GET['gistpen-errors'];
        ?>
  </p>    
</div>

<?php }