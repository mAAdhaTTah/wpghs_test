---
ID: 2736
post_title: ParseCSV and PHP Curling
author: James DiGioia
post_date: 2014-04-29 12:24:32
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/parsecsv-php-curling/
published: true
---
One of the things that came out of the [BatchYouTubeUploader][1] was this nifty little object class, [ParseCSV][2], which I use to manipulate the CSV file for uploading the videos. I know it's going to find a lot of use, and in this case, I wrote this little script to loop through a CSV and download a bunch of images. Just note you have to have `allow_url_fopen` set to `true`, as per this [StackExchange][3] post.

[gistpen id="2814"]

 [1]: http://jamesdigioia.com/new-project-batchyoutubeuploader/
 [2]: https://github.com/jimeh/php-parsecsv
 [3]: http://stackoverflow.com/questions/724391/saving-image-from-php-url-using-php