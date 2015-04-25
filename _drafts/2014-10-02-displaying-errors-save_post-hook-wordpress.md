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
One of the last things I had to do before [launching WP-Gistpen 0.4.0][1] was go through the codebase and make sure I was handling errors correctly. Internally to the plugin, I first double checked that everything was kicking up errors rather than continuing on,[^1] but I then had to figure out how to display those errors to the user so they could either do something different about it, or report enough information to me so I could fix the problem.

This was easy when dealing with the AJAX side of things: Using `wp_send_json_error` or `_success` respectively and managing that in the front-end JavaScript side was fairly easy.[^2] However, the other part of making this work was to report errors generated in the `save_post` hook, which, because WordPress doesn't include any flash message handling, is actually quite a bit more complicated than it looks.

The reason for this is due to the way WordPress handles saves. If you're on a page like `post.php?post=1234` and you make your edits and hit save, WordPress `POST`'s the information to the `post.php`. After the `save_post` hook fires, it *redirects* back to the `post.php?post=1234` with some extra information, i.e. `&message=1`.

This makes things difficult because the redirect clears any global variables or class properties from memory, so we need to persist that data somehow. In my investigation, I found 3[^7] potential methods of solving this problem. I'll walk you through them all, point out some pros and cons of each, and finally explain which one I used and why.

# Some Reference Boilerplate

Before we get started, I'm going to be using the `admin_notices` hook for error displaying, which looks like this:

[gistpen id="3152"]

I'll detai the `my_error_messages` function for each method as we go.

I'm also ignore whatever you decide to do in the `save_post` hook and just assume you end up at the end of the hook with a variable called `$errors`, which is an array of either error messages or codes, depending on which method you choose. This variable will be saved in various ways and displayed with the `my_error_messages` function. So assume something like this:

[gistpen id="3161"]

I'm not going to include the full `save_post` hook function, nor any manipulating of `$errors` except what you see above. I'm going to assume we're going to get to the end of `save_post` with a `$error` variable that can be either an empty array, which we'll check for, or an array of error messages or codes.

# Manipulating `$_SESSION`s

Early in your PHP career, when you're learning about the superglobals like `$_GET` and `$_POST`, you may also learn about `$_SESSION`[^3]. Because the `$_SESSION` superglobal carries over within a single session, we can save our errors to it and pull from them to display them.

## How It Works

First, you'll have to initiate the session, as WordPress doesn't use sessions on its own[^4]:

[gistpen id="3150"]

This should go somewhere early in your plugin, although I think you can start the session in the `save_post` hook itself if you'd like. I didn't use this solution, but that's where [this StackExchange answer][2] suggests it should go. Your milage may vary.

I wouldn't follow that answer exactly, however. Here's how I would do it:

[gistpen id="3164"]

Then, in the `admin_notices` hook:

[gistpen id="3166"]

That will loop through the error and output them above the page title.

## Pros and Cons

On the plus side, this solution is fairly easy to implement and doesn't require you to hit the database, which the next two solutions do. The major problem with it, as alluded to earlier, is that WordPress doesn't use sessions. It is intended to be "stateless," so it doesn't maintain that information in that way as it proceeds through its operations, which could causes problems for some users on particular hosts. As [popsi explained in the answer to that question][3], "people will hate you for it." Fundamentally, it's not "The WordPress Way" and runs counter to the philosphy of the project, but it is an easy and lightweight solution to the problem if you're building a tool for yourself or a client, where you have full control over the environment.

# Saving as a Transient

Transients are a WordPress method of short-term caching, basically. They have an expiration time and aren't intended to hold long-term bits of information, but they can be very useful in this context.

## How It Works

We don't need to initiate anything to get started with transients. When you have your errors array, just set a transient with the information:

[gistpen id="3147"]

[Here's the reference for that function][4]. Just note that the last param is the number of seconds that the transient will last for. 45 seconds should be enough time to save and get the data later, and if it isn't, you're doing something verrrrry wrong with your WordPress install.

Then, in the `admin_notices` hook:

[gistpen id="3167"]

Similar loop as above. Don't forget to delete the transient, though it'll get deleted on its own in 45 seconds.

## Pros and Cons

On the plus side, this is certainly a WordPress-friendly way of managing these errors, and transients are designed for caching stuff like this, so the data will be cleared out on its own, even if you forget to clear it yourself. The primary problem with this solution is it has to hit the database, so if you don't want to add another query to your plugin, you'd want to avoid this method.

I'll also add you can use this same idea with `post_meta`. I was told this doesn't add an extra query, as it just adds a statement to the query for the post, but I'm not entirely sure that's the case. My view on it is that you shouldn't use `post_meta` data for temporary data anyway, but if someone can confirm that's true, I'll update this post.

# Adding a `GET` Param to the Redirect

This ended up being the method I went with, and it's actually the way WordPress shows its "Post Updated" message. Here's how that works:

First of all, you're going to want to use error codes instead of full error messages. As you fill in your array, if you're using `WP_Error` objects, use the `get_error_code` method instead, which will be a shorter version of error than a full message. Because we're passing via the `$_GET` param, we don't want to pass super long strings up there or the URL is going to look dopey as hell.

So at the end of our `save_post` hook, let's check for errors, and if so, add them to the URL parameter:

[gistpen id="3145"]

In this case, we're using an anonymous function with the `use` statement so we can reuse the `$errors` variable in the function, and we have to convert the array to a string with `implode`.[^5]

Then, we display the error codes in the admin_notices hook:

[gistpen id="3169"]

## Pros and Cons

The big plus on this is you never have to hit the database. All of the error codes are stored and retrieved in memory, which makes it much more performant than the others.[^6] The negative side is you're limited in this case to passing a string. This limits the amount of information that can be passed through this method, but works well enough if you have simple errors and good error codes.

# Conclusion

As I mentioned, I used the 3rd method in WP-Gistpen. You can find the information about the errors in the [Saver class][5] and the display in the [Editor class][6].

Hope that helps. If you have any questions, leave a comment below.

[^1]:    
    I actually ran into an issue where, because of an unrelated bug, the `query` class was getting malformed data back from one of its operations and instead of kicking the error up the chain like it should, it was inserting the malformed data into the object it was building, which caused all sorts of headaches on the front-end until I figured out where the actual issue was. It's also the reason I didn't release last weekend, as I realized my error handling was not up-to-par either.

[^2]:    
    For right now, I'm just `console.log`'ing a lot of that stuff, which obviously isn't the best way to do it, but given that I'm dealing primarily with developers as my audience, I'm assuming they know what the console is and can pull the error message for me to troubleshoot. I'll probably release better error handling in 0.5.0, maybe 0.4.1 if I get more issues that I bargained for.

[^7]:    
    Technically 4, but I combined `transients` and `post_meta` into one section because the idea for both is similar.

[^3]:    
    If you didn't, then you're a bad developer. Kidding! It happened to be covered in one of the early video lessions I watched, but if you haven't learned it, [here's the reference in the PHP manual,][7] and [here's a good tutorial on how to use it.][8]

[^4]:    
    Which, as you'll see shortly, is one of the problems with this solution.

[^5]:    
    Just note: this is NOT compatible with PHP 5.2, and WordPress is compatible with 5.2+, so if this is an issue for you, choose another solution.
    
    The other possibility is what I did, which is to do this all in an object-oriented fashion and save the errors as a class property which is accessed later. I won't go through the details of that here because this tutorial is functional, not object-oriented.

[^6]:    
    Transients will work as in-memory as well if the user has memcached or some other caching plugin installed, although we can't assume that's the case.

 [1]: http://jamesdigioia.com/wp-gistpen-0-4-0-released/
 [2]: http://wordpress.stackexchange.com/a/19980
 [3]: http://wordpress.stackexchange.com/questions/5102/add-validation-and-error-handling-when-saving-custom-fields/19980#comment84922_19980
 [4]: https://developer.wordpress.org/reference/functions/set_transient/
 [5]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/develop/admin/includes/class-wp-gistpen-saver.php
 [6]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/develop/admin/includes/class-wp-gistpen-editor.php
 [7]: http://php.net/manual/en/reserved.variables.session.php
 [8]: http://www.sitepoint.com/php-sessions/