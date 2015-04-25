---
ID: 4219
post_title: How to Use ngrok with Bedrock-Ansible
author: James DiGioia
post_date: 2015-03-07 16:08:15
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/how-to-use-ngrok-with-bedrock-ansible/
published: true
---
Now is a pretty awesome time to be getting into web development. With [Node][1] blowing up in popularity, and the PHP world rallying around [composer][2], the tooling available for web development are getting really quite cool these days. Because of its legacy status, WordPress is still a bit behind the times on these things, but there are developers trying to bring these practices into WordPress development, and I've been really excited to follow the work done by the [Roots][3] team. I've been using [bedrock][4] and its Vagrant setup counterpart, [bedrock-ansible][5], for pretty much all my projects now.

I've been working on [WordPress <--> GitHub Sync][6] lately, which required testing GitHub's Webhooks with the plugin. As [suggested by GitHub][7], I used ngrok to test this set up, but it required some manual work every time I wanted to do it.

Because the Vagrant box is set up with Ansible, you can set it up to do all that manual work for you. This tutorial will show you how to integrate `ansible-ngrok` into your local bedrock-ansible development environment.

## Walkthrough

To start, I'm going to assume you have a bedrock-ansible development environment that is set up and working. If you don't, check out the [documentation][8].

Setting up ngrok is as easy as adding a few lines to your current configuration. First, add this line to `requirements.yml`:

    - name: ngrok
      src: mAAdhaTTah.ngrok
    

Next, add this line to your `dev.yml` file:

    - { role: ngrok, tags: [ngrok] }
    

Finally, edit your `group_vars/development` file to include this line:

    ngrok:
      subdomain: domain
      token: your_token
    

Where `domain` will become `domain.ngrok.com` for your site, and `your_token` is the auth token found on your [dashboard][9]. This isn't required for ngrok, per se, but is required if you want to set a hostname, which you will need for WordPress.

Lastly, you need to change `site_name`, `site_url`, `wp_home`, and `wp_siteurl` from whatever local url you've been using to `domain.ngrok.com`, again, where `domain` is the subdomain set above.

Once you've done that, run `ansible-galaxy install -r requirements.yml -p vendor/roles --ignore-errors` to install ngrok from ansible-galaxy. Before running `vagrant up`, add `config.vm.network "public_network"` under `config.vm.network :private_network, ip: '192.168.50.5'` in your `Vagrantfile`. After the Vagrant box is completely provisioned, ssh into the box (`vagrant ssh`) and run `ngrok`, and the ngrok client will begin running at the set url.

Open that url in your browser, and you're all set! Now you can show your local development environment to clients and test webhooks with ease.

 [1]: http://nodejs.org/
 [2]: https://getcomposer.org/
 [3]: http://roots.io
 [4]: https://github.com/roots/bedrock
 [5]: https://github.com/roots/bedrock-ansible
 [6]: https://github.com/benbalter/wordpress-github-sync
 [7]: https://developer.github.com/webhooks/configuring/
 [8]: https://github.com/roots/bedrock-ansible/blob/master/README.md
 [9]: https://ngrok.com/dashboard