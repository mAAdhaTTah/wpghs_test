---
ID: 3119
post_title: >
  Displaying Errors from the save_post
  Hook in WordPress
author: James DiGioia
post_date: 2014-10-02 19:44:38
post_excerpt: ""
layout: post
permalink: http://jamesdigioia.com/?p=3119
published: false
---
Despite the existence of `WP_Error` and all the pieces required, this is no single "WordPress Way" to handle and display errors. You can indicate an error occurred in a function or method by returning said `WP_Error` object (and that's been my usual MO in my work), but once we do, how do we handle these error objects?

It's easy on the AJAX side of things: Use `wp_send_json_{error/success}` and handle the response accordingly. However, a common area where generated errors need to be displayed to the user is on the `save_post` hook, which is actually slightly more complicated than it looks.

The reason for this is due to the way WordPress handles saves. If you're on a page like `post.php?post=1234` and you make your edits and hit **save**, WordPress `POST`'s the information to `post.php`. After the `save_post` hook fires the everything that's done is done, it *redirects* back to the `post.php?post=1234` editor page.

This makes things difficult because the redirect means the execution thread for loading the editor page isn't the same as the thread for saving the post, and we no longer have access to the same global variables we did when we were saving. Because of this, we need a way to pass that information from the action to the page we're being redirected to.

Let's walk through 3 potential methods of solving this problem. I'll explain how to implement each one, point out some of their pros and cons, and explain to which contexts they're best suited.

# Some Reference Boilerplate

Before we get started, I'm going to be using the `admin_notices` hook for error displaying, which looks like this:

[gistpen id="3152"]

This hook gives us a place to display messages above the page title. I'll detail the `my_error_message` function for each method as we go. I'm also going to ignore whatever you decide to do in the `save_post` hook and just assume you end up at the end of the hook with an `$error`, which is either `false` if there were no errors or a `WP_Error` object if there was. The execution flow would look something like this:

[gistpen id="3161"]

Something happens with the `$post_id` (not pictured), and if something goes wrong, we'll create a `WP_Error` object which we'll handle.

# Save it in the `$_SESSION`

PHP (and WordPress, actually) is (in)famous for its globals. Similar to how the `$_GET` and `$_POST` variables hold the data for those HTTP verbs, the `$_SESSION` global holds the data for the current session[^1]. The `$_SESSION` data is persisted in the configured session storage (e.g. memcache, Redis, filesystem) and tied to the user's session through a cookie. We can use that to save and retrieve our error message.

## How It Works

First, you'll have to initiate the session, as WordPress doesn't use sessions on its own:

[gistpen id="3150"]

This should be hooked in early in the WordPress lifecycle, although I think you could start the session in the `save_post` hook itself if you'd like. Then, we save the error to the session:

[gistpen id="3164"]

Finally, in the `admin_notices` hook:

[gistpen id="3166"]

we pull the error message from the session and output it above the page title.

## Pros and Cons

On the plus side, this solution is fairly easy to implement and doesn't require you to hit the database, which the other two solutions do. The major problem with it is: *WordPress doesn't use sessions*. It's designed to be "stateless," so it's not designed to maintain information as it proceeds through its operations, which will causes problems for some users if they don't have a session store. Fundamentally, it's not "The WordPress Way" and runs counter to the philosophy of the project, but it is an easy and lightweight solution to the problem if you're building a tool for yourself or a client, where you have full control over the environment.

# Saving it in a Transient

Transients are a WordPress caching API. It allows you to save any kind of information with an expiration date, like you would in any other cache. Under the hood, the Transient API will attempts to save your data to the object cache, *if you have a persistent cache enabled*. If you don't, it will save your data to the `wp_options` table. This way, no matter how the site's back-end is configured there's a (somewhat) reliable API for persisting short-term data. It abstracts out a lot of hard work for you.

One word of caution: While the data should be there (and for the short period of time we're saving it for, it almost always will be) when we get it, there are reasons it could get lost, especially of it's being saved to the object cache. You can't assume the data will always be there. If you absolutely need the information to persist, you should save it to the database directly.

## How It Works

We don't need to initiate anything to get started with transients. When you get an error, just set a transient with the information (you'll need to fetch the `$user_id` in advance):

[gistpen id="3147"]

[Here's the reference for that function][1]. Just note that the last param is the number of seconds that the transient will last for. 45 seconds should be enough time to save and get the data, and if it isn't, something has gone horribly wrong.

Then, in the `admin_notices` hook:

[gistpen id="3167"]

we grab the `WP_Error` from the cache and display its message. Don't forget to clean up after yourself!

## Pros and Cons

On the plus side, this is a WordPress-friendly way of managing these errors, designed for caching stuff like this. The data will be cleared out on its own, even if you forget to clear it yourself, so it provides a kind of sanity check for you, wiping or ignoring stale error messages. The primary problem with this solution is it has to hit the database if you don't have an object cache installed, so if you're doing a lot of work in the `save_post` hook and don't want to add another query, this may not be the ideal solution.

I'll also mention you can use this same idea with `post_meta`. If I recall correctly, this may not add an extra query, as it just adds a statement to the query for the post, but I'm not sure. Using `post_meta` for temporary data isn't really it's ideal purpose, but if it gives you a performance boost, it may be worth it. Definitely make sure to clean up after yourself then, since that won't be done for you with `post_meta` the way `transients` do.

# Adding a `GET` Param to the Redirect

This third method is actually the way WordPress shows its "Post Updated" message: with a `$_GET` param. In WordPress's instance, it sets the `message` value to a number, and uses that number to display a particular message. You can do something similar, adding the `WP_Error`'s code, instead of its full message, to the url as a query var.

At the end of our `save_post` hook, we add the error code to the URL parameter:

[gistpen id="3145"]

Anonymous functions weren't ADDED until PHP 5.3, and WordPress supports 5.2+, so their use here may disqualify you from this method, but this is a pretty ideal use case for them, pulling in a variable from its context and using it in the filter.

Then, we display the error message, based on its code, in the admin_notices hook:

[gistpen id="3169"]

## Pros and Cons

The big plus on this is you never have to hit the database. All of the error codes are stored and retrieved in memory, which makes it much more performant than the others. The negative side is you have to duplicate the messages in both the instantiation of the `WP_Error` object and the switch statement. You can't use a constant or a set of variables or anything like that because then the strings can't be translated. If you have a limited number of error codes, this shouldn't be an issue, but if your application gets large, it could be more difficult to maintain long-term.

# Conclusion

Each of these three methods are better or easier to implement in various situations. I like to use transients to pass data around across page loads, especially when displaying error messages the user needs to see and possibly react to. The query variable has the lowest overhead of all three, and works great when your application only generates a small number of errors. And while the session isn't really supported by WordPress, it is a common method for flashing messages to the user in other applications.

I hope that these potential approaches to displaying error messages have been useful to you. Let me a comment if you have any questions.

[^1]:    
    [The reference in the PHP manual,][2] and [here's a good tutorial ons it.][3]

 [1]: https://developer.wordpress.org/reference/functions/set_transient/
 [2]: http://php.net/manual/en/reserved.variables.session.php
 [3]: http://www.sitepoint.com/php-sessions/
