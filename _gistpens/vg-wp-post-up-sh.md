---
ID: 3154
post_title: vg-wp-post-up.sh
author: James DiGioia
post_date: 2015-02-14 18:36:38
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/vagrant-wordpress-post-up-for-wp-gistpen/vg-wp-post-up-sh/
published: true
---
sudo apt-get -y install subversion phpunit cd /srv/www/jamesdigioia.dev/current cd $(wp plugin path --dir wp-gistpen) bash test/install-wp-tests.sh wordpress_test root devpw localhost latest