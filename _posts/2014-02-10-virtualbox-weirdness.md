---
ID: 2546
post_title: VirtualBox Weirdness
author: James DiGioia
post_date: 2014-02-10 12:06:03
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/virtualbox-weirdness/
published: true
---
I'm getting close to sharing version 0.0.1 of WP-Gitdown, a feature-incomplete version of the project. I'm going to just get something up ASAP because I'm still learning PHP (and AJAX, apparently) and would love feedback on things I'm doing right or wrong. However, I did want to share one oddity I've already run into using Vagrant.

One of the steps the plugin takes upon activation is creating the git repo we're going to store all our posts in. In testing, I delete the git repo from the *host* side (in my case, OS X), and deactivate/reactivate the plugin to test if the folder gets recreated. And unfortunately, it wasn't.

I actually thought it was an error in the Wordpress function, [`wp_mkdir_p`][1], which was basically "seeing" the folder as created, but returning `false` for `is_dir` ([line 1362][2]). Which was really weird. I actually opened a question in Stackexchange's Wordpress community before eventually tracking it down to a [VirtualBox bug][3], which apparently only affects guests with a Linux kernel. I was using VagrantPress as my base to run from, so Ubuntu had the problem.

A quick `vagrant reload` solves the problem.

 [1]: http://codex.wordpress.org/Function_Reference/wp_mkdir_p
 [2]: https://core.trac.wordpress.org/browser/tags/3.8.1/src/wp-includes/functions.php#L0
 [3]: https://www.virtualbox.org/ticket/9197