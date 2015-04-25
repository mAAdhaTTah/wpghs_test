---
ID: 3458
post_title: test.php
author: James DiGioia
post_date: 2015-02-14 18:35:26
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/default-classes-for-wp-gistpen/test-php/
published: true
---
<?php

class WP_Gistpen_Sample_Test extends WP_Gistpen_UnitTestCase {

	function setUp() {
		parent::setUp();
	}

	function test_sample() {
		// replace this with some actual testing code
		$this->markTestIncomplete( "This test is incomplete." ); } function tearDown() { parent::tearDown(); } }