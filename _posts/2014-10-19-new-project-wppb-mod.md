---
ID: 3216
post_title: 'New Project: wppb-mod'
author: James DiGioia
post_date: 2014-10-19 19:30:13
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/new-project-wppb-mod/
published: true
---
If you're a WordPress developer working on your own plugins, you may have come across [Tom McFarlin's WordPress Plugin Boilerplate][1]. I used its previous iteration to start [WP-Gistpen][2], and I've got another plugin or two in the works that's also based on it. The latest version was a major upgrade, with better organization and lot of excess code removed. I like it a lot, but as WP-Gistpen got more advanced, the public/admin/includes structure became too limiting. I rearranged the Boilerplate's folder structure, added my toolset, and registered the resulting package on [Packagist][3].

Meet [wppb-mod][4].

My favorite part about this is you can run `composer create-project maadhaattah/wppb-mod <folder>` and you have a good structure to get started. This also means we can add a script that will find/replace the `Plugin Name` and related strings in the files as you run the composer command, something similar to [this][5], making the whole process of starting a new plugin super simple.

Check it out and let me know what you think!

 [1]: https://github.com/tommcfarlin/WordPress-Plugin-Boilerplate
 [2]: http://jamesdigioia.com/wp-gistpen/
 [3]: https://packagist.org/packages/maadhattah/wppb-mod
 [4]: https://github.com/mAAdhaTTah/wppb-mod
 [5]: https://github.com/hasinhayder/plugin-boilerplate-code-generator/blob/master/index.php