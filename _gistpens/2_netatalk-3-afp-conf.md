---
ID: 4192
post_title: 2_netatalk-3-afp.conf
author: James DiGioia
post_date: 2015-02-15 20:10:41
post_excerpt: ""
layout: gistpen
permalink: >
  http://jamesdigioia.com/gistpens/shell-script-to-install-netatalk-3-on-ubuntu-14-04/2_netatalk-3-afp-conf/
published: true
---
;/usr/local/etc/afp.conf ; Netatalk 3.x configuration file ; [Global] ; Global server settings vol preset = default_for_all_vol hostname = TimeCapsule log file = /var/log/netatalk.log log level = default:info uam list = uams_dhx.so,uams_dhx2.so save password = no disconnect time = 168 dsireadbuf = 96 sleep time = 24 tcprcvbuf = 524288 tcpsndbuf = 524288 dircachesize = 131072 keep sessions = yes mimic model = Xserve [default_for_all_vol] file perm = 0664 directory perm = 0774 ;cnid scheme = cbd valid users = @tm [Homes] basedir regex = /home cnid scheme = dbd home name = Home: $u [TimeMachine] path = /home/tm time machine = yes ;vol size limit = 953674