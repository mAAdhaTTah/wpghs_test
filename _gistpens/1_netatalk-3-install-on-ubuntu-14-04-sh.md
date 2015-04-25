---
ID: 4191
post_title: 1_netatalk-3-install-on-ubuntu-14.04.sh
author: James DiGioia
post_date: 2015-02-15 20:10:41
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/shell-script-to-install-netatalk-3-on-ubuntu-14-04/1_netatalk-3-install-on-ubuntu-14-04-sh/
published: true
---
# Get root: sudo su # Install prerequisites: apt-get install build-essential pkg-config checkinstall git avahi-daemon libavahi-client-dev libcrack2-dev libwrap0-dev autotools-dev automake libtool libdb-dev libacl1-dev libdb5.1-dev db-util db5.1-util libgcrypt11 libgcrypt11-dev # Build libevent from source: cd /usr/local/src wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz tar xfv libevent-2.0.21-stable.tar.gz cd libevent-2.0.21-stable ./configure make checkinstall --pkgname=libevent-2.0.21-stable --pkgversion="$(date +%Y%m%d%H%M)" --backup=no --deldoc=yes --default --fstrans=no cd ../ # Download src: git clone git://git.code.sf.net/p/netatalk/code netatalk-code cd netatalk-code ./bootstrap # Configure install ./configure --enable-debian --enable-zeroconf --with-cracklib --with-acls --enable-tcp-wrappers --with-init-style=debian make # Build! checkinstall --pkgname=netatalk --pkgversion="$(date +%Y%m%d%H%M)" --backup=no --deldoc=yes --default --fstrans=no # Config is in /usr/local/etc/afp.conf