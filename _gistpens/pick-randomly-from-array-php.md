---
ID: 3153
post_title: pick-randomly-from-array.php
author: James DiGioia
post_date: 2015-02-14 18:36:21
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/algo-to-pick-every-item-from-array-once/pick-randomly-from-array-php/
published: true
---
 $num_posts = count( $languages ); $this->gistpens = $this->factory->post->create_many( $num_posts, array( 'post_type' => 'gistpens', ), array( 'post_title' => new WP_UnitTest_Generator_Sequence( 'Post title %s' ), 'post_name' => new WP_UnitTest_Generator_Sequence( 'Post title %s' ), 'post_content' => new WP_UnitTest_Generator_Sequence( 'Post content %s' ) )); foreach ( $this->gistpens as $gistpen_id ) { // Pick a random language $num_posts = $num_posts - 1; $lang_num = rand( 0, ( $num_posts ) ); // Get the language's id $lang_id = $languages[$lang_num]; // Remove the language and reindex the languages array unset( $languages[$lang_num] ); $languages = array_values( $languages );