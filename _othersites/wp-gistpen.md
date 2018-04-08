# WP-Gistpen: A Gist Clone for WordPress

[WP-Gistpen](https://wordpress.org/plugins/wp-gistpen/) is a WordPress plugin for saving and displaying code snippets on your WordPress site. I started this project because I believe in controlling your own data, even if it's something as simple as a code snippet. They say developers loathe a 1.0, and after three years of development, that rings more true than ever. But it's finally here, and it's pretty great.

I started building this plugin before Facebook was known for undermining democracy and enabling invasion by foreign state actors, but even at the time, the sheer quantity of data they were collecting was concerning. Code snippets provides one small area to reclaim your data, but we should be building tools that make it as easy as possible to do that. For all its flaws, WordPress has an admirable dedication to user experience and ease-of-use, so it makes a solid platform for building these sorts of tools, and I learned a lot building WP-Gistpen.

For that purpose, WP-Gistpen uses the lightweight [Prism](https://github.com/PrismJS/prism/) by Lea Verou for syntax highlighting on the web-facing side. Originally, the editor was built with the [ACE editor](https://ace.c9.io/), but I wanted harmony between the editing & display experience, so I built a custom editor on top of Prism. I'm in the process of moving it out of a custom view layer into React, open-sourcing it, and ironing out any remaining bugs.

The editor screen is entirely API-driven, integrating with the WP-API to enable a seamless editing experience. Each of the other screens in the WordPress admin are also API-driven, including the settings screen as well as a TinyMCE plugin for injecting shortcodes, which you can use to embed your code snippets into your posts. You can also embed them on other sites using WordPress' built-in oEmbed, which has been customized for WP-Gistpen code snippets.

Lastly, you can import & export your code snippets with GitHub Gist. I built this feature specifically because I don't want to lock your data into your system. You can migrate your current Gists into WP-Gistpen, export new snippets to Gist, and otherwise access your code snippets via API. I want to keep it easy to use and extend, so everything related to the plugin is exposed via API.

If you're interested, you can check out [WP-Gistpen on wordpress.org](https://wordpress.org/plugins/wp-gistpen/) or [check out the source on GitHub](https://github.com/intraxia/wp-gistpen). Play around it it and file any issues you come across, and help me make it better.

And if you're trying to decide what your next project should be, build something to host your own data. The future thanks you for it.
