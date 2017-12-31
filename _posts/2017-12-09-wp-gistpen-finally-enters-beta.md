---
ID: 5429
post_title: WP-Gistpen (finally!) enters beta
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/wp-gistpen-finally-enters-beta/
published: true
post_date: 2017-12-09 16:53:12
---
It's been more than 2 years in the making, but I've finally got [WP-Gistpen][wpgp], my code snippet WordPress plugin, into a state where I feel comfortable putting it out into the world, and I'm looking for beta testers. If you're interested and have a WordPress site you can use it on, check out and comment on this [GitHub issue][gh-issue]. Everything you need to get started can be found there.

If you're not already aware, WP-Gistpen is a WordPress plugin for saving your code snippets to WordPress. It's essentially a Gist clone for WordPress, backed by Prism and a custom Prism-based code editor. WP-Gistpen also syncs with Gist, allowing you to manage your Gist account from WordPress.

WP-Gistpen turned into an interesting exploration of a lot of different approaches, and if you're a developer, there are several projects you can get involved with. On the back-end, the plugin uses a small framework I built called [`jaxion`][jaxion], which provides a structured object-oriented approach to building WordPress plugins. It provides a basic App Container and a Loader for binding classes to WordPress hooks. I also built heavily on the WP-API, and all the plugin's pages are API-driven apps. On the front-end, I'm using the Handlebars templating language along with [`brookjs`][brookjs], a React-inspired front-end framework I've been working on. WP-Gistpen takes advantage of the work on both of those projects, so you can work on either framework or an application that uses both.

There are certainly [... bugs ...][bugs] but the data the on the back-end should be solid. As the plugin is still in beta, **please back up your database**. I have the plugin running on my live site, but there's always a chance something could go wrong, especially if you're upgrading from the .org version.

If you're interested in putting this plugin through its paces, comment on [GitHub][gh-issue].

Thanks for checking it out.

  [wpgp]: https://github.com/mAAdhaTTah/wp-gistpen
  [gh-issue]: https://github.com/mAAdhaTTah/wp-gistpen/issues/142
  [jaxion]: https://github.com/intraxia/jaxion
  [brookjs]: https://github.com/valtech-nyc/brookjs
  [bugs]: https://github.com/mAAdhaTTah/wp-gistpen/issues?q=is%3Aopen+is%3Aissue+label%3Abug