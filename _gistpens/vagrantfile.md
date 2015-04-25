---
ID: 4274
post_title: vagrantfile
author: James DiGioia
post_date: 2015-04-11 15:41:06
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/mount-arbitrary-directory-with-bedrock-ansible/vagrantfile/
published: true
---
config.vm.synced_folder '../themes/sage', File.join(nfs_path(name), 'web/app/themes/sage'), type: 'nfs'