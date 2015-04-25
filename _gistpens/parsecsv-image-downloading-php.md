---
ID: 3160
post_title: parsecsv-image-downloading.php
author: James DiGioia
post_date: 2015-02-14 18:37:20
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/parsecsv-image-downloading/parsecsv-image-downloading-php/
published: true
---
require("ParseCSV.php"); $csv = new ParseCSV("youtube-images.csv"); $n = 1; foreach($csv->data as $data) { if(false != file_put_contents("pics/{$data['filename']}.jpg", file_get_contents($data['url']))) { $log = "File {$n}: Downloaded {$data['filename']} from {$data['url']}n"; } else { $log = "File {$n}: Download from {$data['url']} failedn"; } print $log; file_put_contents("output.log", $log, FILE_APPEND | LOCK_EX); $n++; }