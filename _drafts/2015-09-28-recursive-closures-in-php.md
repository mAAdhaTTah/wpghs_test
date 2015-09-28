---
ID: 4576
post_title: Recursive Closures in PHP
author: James DiGioia
post_date: 2015-09-28 17:50:21
post_excerpt: ""
layout: post
permalink: http://jamesdigioia.com/?p=4576
published: false
---
I don't know how often this comes up, but the other day, I ran into a situation where I needed to create a one-off function I could use recursively. In this case, I had a set of items that were both nested and could be disabled at any level. So you could have a 3 widgets, all of a type "useful", and all of the "useful" level widgets were then grouped by "kind"[^1]. So I could disable a single widget by date, any widget "type", or even a widget "kind". So like this:

             "kind"
              /
           "type"
       /     |     \
    widget widget widget
    

Insofar as that graph has any meaning to anyone. Imagine this extending outwards, so there were many "types", each with 2 or 3 widgets, and each "kind" then has 2 or 3 "types". If any "type" has no widgets, it too gets disabled; same for "kind." And obviously, the total universe of things to sell has many kinds.

So we need to go through each level, and check both that each item (a widget, a type, or a kind), is both active (as per the dates set) as well as has children (except for the widget level). If it's no longer active, we need to remove it as well as all of its children from being displayed on a webpage.

One solution is to just create a method for this purpose to call itself, but then you have to go off to another location to look at the code, which isn't particularly useful if you're only using the method in this one context. And the method is really small.

Another option is use a closure. In JavaScript, it's pretty easy to write one of these, as the variables within the `function`'s scope are accessible when the function is run:

    var remove = function(pageId) {
        pages[pageId].getChildPageIds().forEach(function(childPageId) {
            remove(pages[childPageId]);
        });
    
        delete pages[$pageId];
    };
    

But we can't do this as directly in PHP, because we don't have closure scoping, and its closest equivalent, the `use` statement, passes by value, and when that statement is executed, `remove` doesn't exist yes, so it'll throw a warning. In order to get around this, we need to pass remove *by reference*:

    $remove = function($pageId) use (&$pages, &$remove) {
        foreach ($pages[$pageId]->getChildPageIds() as $childPageId) {
            $remove($pages[$childPageId]);
        }
    
        unset($pages[$pageId]);
    };
    

This passes the reference to `$remove`, rather its value. At the time the closure is defined, the reference isn't to *anything*, actually. But when the closure is executed, the reference is now to the actual closure[^2].

[^1]:    
    Specifics redacted to protect the guilty.

[^2]:    
    Which, curiously, is an object in PHP, with the magic method `__invoke` defined as the closure.