---
ID: 3088
post_title: >
  Creating Your Own WordPress Unit Test
  Factories
author: James DiGioia
post_date: 2014-09-29 15:32:52
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/creating-wordpress-unit-test-factories/
published: true
_format_link_url: >
  http://codesymphony.co/creating-your-own-wordpress-unit-test-factories/
---
I did something like this in developing the tests for WP-Gistpen. In my case, I took the default `post` factory, as he details it, and changed the `post_type` to 'gistpen'. I was actually going to write a thing about it myself, but he got to it first, so I'm just going to repost his. You can see my factory [here][1].

One of the things I'll add that wasn't mentioned in the original article is that you might want to add your new factory as part of the test unit's general factory, which is what the first class does. I then extend the [testunit case][2] to insert my new factory, as well as add a few helper functions I used throughout the test.

 [1]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/develop/test/includes/factory.php
 [2]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/develop/test/includes/testcase.php