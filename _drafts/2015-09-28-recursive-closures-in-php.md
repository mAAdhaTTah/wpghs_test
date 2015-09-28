---
ID: 4576
post_title: Recursive Closures in PHP
author: James DiGioia
post_date: 2015-09-28 17:37:12
post_excerpt: ""
layout: post
permalink: http://jamesdigioia.com/?p=4576
published: false
---
I don't know how often this comes up, but the other day, I ran into a situation where I needed to create a one-off function I could use recursively. In this case, I had a set of items that were both nested and could be disabled at any level. So you could have a 3 widgets, all of a type "useful", and all of the "useful" level widgets were then grouped by "kind"[^1]. So I could disable a single widget by date, any widget "type", or even a w

[^1]:    
    Specifics redacted to protect the guilty.