---
ID: 2541
post_title: 'New Project: WP-Gitdown'
author: James DiGioia
post_date: 2014-02-03 10:46:35
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/new-project-wp-gitdown/
published: true
---
Over the weekend, I finally started working on an idea I had, borne out of Ben Balter's [GitHub for Journalism][1] post and the resulting [Post Forking][2] plugin. While this is an awesome idea, there were some problems I had using it myself:

1.  I have concerns with forking the posts within the database and potential problems with merging there[1. Although admittedly that's partially a result of my lack of experience with WordPress as well as not taking a particularly close look at their plugin.]
2.  I wanted .md copies of my posts anyway for backup

Overall, it's a great project; I don't mean to throw shade on their work. However, they do basically have to rebuild Git's functionality within a WordPress context, including the database interactions and whathaveyou, and it looks like a lot of work to make sure it works correctly.

Instead of that, WP-Gitdown exports the posts as Markdown files, commit them to an actual Git repo, which gets pushed and pulled to from GitHub and read from when you update a post. This avoids the need to build the forking functionality within WordPress.

I got the project started this weekend, and currently it so far has a button that exports all the posts to .md files and commits them to a repo. When I've got some more basic functionality (and less errors), I'll be putting this up on GitHub for contributions.

 [1]: http://ben.balter.com/2012/02/28/github-for-journalism-what-wordpress-post-forking-could-do-to-editorial-workflows/
 [2]: https://github.com/post-forking/post-forking