---
ID: 2880
post_title: 'How to add a button with a pop-up in WordPress&#8217;s TinyMCE editor'
author: James DiGioia
post_date: 2014-08-04 14:20:36
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/add-button-pop-wordpresss-tinymce-editor/
published: true
---
The main new feature in version 0.2.0 of WP-Gistpen was the integration of the Gistpen button into WordPress's TinyMCE editor. Since TinyMCE was upgraded to version 4 in WordPress 3.9, many of the current tutorials are no longer entirely accurate. Additionally, TinyMCE's documentation is shit, so I just want to share how I integrated a button that triggers a pop-up into TinyMCE 4.

First all, you can find the Gistpen-specific code on Github. All the PHP stuff is [here][1], and the JavaScript is [here][2], if you want to see the specific implementation.

First, register the plugins and the buttons in WordPress on the proper filters:

[gistpen id="2896"]

The [`mce_external_plugin`][3] filter registers the plugin, and the [`mce_buttons`][4] filter registers the button. Just swap out your plugin slugs and the path to the JavaScript file where you're writing your TinyMCE plugin.

Here's the TinyMCE plugin code itself. Take a quick look over the whole thing, as it's a bit complicated, but we'll go through all of the parts step-by-step:

[gistpen id="2900"]

First, we create the plugin on [line 4][5]. Don't forget to swap out your plugin slug.

On [lines 9-14][6], we add the button to TinyMCE. You can edit the button text, but it doesn't look great, or you can define its icon class and display an icon there. `cmd` defines what command is run when you click the button.

Next, we [register the command][7] and open the pop-up using TinyMCE's windowManager API.

We can define some aspects of the pop-up using their API. Some of these, like the buttons property and the id property, aren't well documented, but you can see some of the properties [here][8][^1].

TinyMCE has its own window API, and a number of tutorials will show you how to use it to build your own forms, which you can find and use, but I wanted more flexibility for my form. In a future version of the app, I plan to draw the form with jQuery, as it'll be faster than requesting it via Ajax, but this was certainly easier. Here's how this works.

After the pop-up is opened and ready to go, we [call a function][9]. That function makes an [Ajax call][10] that the form and appends the response to the body of the dialog box.

On the WordPress side of the Ajax call, we just have to register a function to that returns the form.

[gistpen id="2916"]

You can then create and return a form in standard PHP!

Using some of these techniques, I actually added a search box into my form, and made it possible to create a new Gistpen from the pop-up. If you want to see how that was accomplished, check out the [WP-Gistpen project on GitHub][11].

[^1]:    
    I'm actually writing this tutorial because of how frustratingly bad the documentation is. For a brief period, I was actually swapping out the button text and giving it a class in jQuery because I didn't realize you could edit it yourself. Read the source next time, I guess.

 [1]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/master/admin/class-wp-gistpen-admin.php
 [2]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/master/admin/assets/js/tinymce-plugin.js
 [3]: #gistpen-register-tinymce-plugin-button.1
 [4]: #gistpen-register-tinymce-plugin-button.7
 [5]: #gistpen-tinymce-button-plugin.4
 [6]: #gistpen-tinymce-button-plugin.9-14
 [7]: #gistpen-tinymce-button-plugin.17
 [8]: http://www.tinymce.com/wiki.php/API3:method.tinymce.WindowManager.open
 [9]: #gistpen-tinymce-button-plugin.42
 [10]: #gistpen-tinymce-button-plugin.56-64
 [11]: https://github.com/mAAdhaTTah/WP-Gistpen