---
ID: 2726
post_title: 'New Project: BatchYouTubeUploader'
author: James DiGioia
post_date: 2014-04-25 09:24:58
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/new-project-batchyoutubeuploader/
published: true
---
At the World Science Festival, we produce a ton of video as a result of all the programs we put on, and part of the process of shoring up our digital presence was to migrate all these videos we had stored on our sever as files to YouTube. And as I said, we produce a **ton** of videos - I had to migrate around 580 videos, and I wasn't going to do them by hand, so I wrote this php command line script to upload them all for me.

I believe I've worked out most of the bugs at this point and should be easy to use. I do assume you have some familiarity with the Google Developer Console, but even that's not that complicated.

You can find the project on [GitHub][1]. Just `git clone` it, fill out the `videos.csv`, and dump your video files into the `videos` folder, and run `php batchyoutubeuploader.php`.

Please submit any bugs you run into. I obviously can't test all use cases, so let me know what problems you run into.

 [1]: https://github.com/mAAdhaTTah/batchyoutubeuploader