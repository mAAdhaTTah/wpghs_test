---
ID: 3303
post_title: This is my website, this is my space
author: James DiGioia
post_date: 2014-11-03 12:38:05
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/this-is-my-website-this-is-my-space/
published: true
---
I finally got around to redesigning my website. It's inspired heavily by [Favepersonal][1], which I was using previously, and it's the first theme I've built with [Foundation][2]. I bundled the same plugins that came with Favepersonal,[^1] so Socialwindow has the same Facebook & Twitter interoperability. This bundled functionality will eventually be replaced by SocialDen, an expansion on the [impetus for WP-Gistpen][3], the desire to maintain control of all my data "in-house;" in this case, on my blog

Part of the problem with [new social networks like Ello][4], and the reason Facebook enjoys its continued dominance, is your average network effect. I don't know that many people on Ello, and those I do know aren't using it as actively as other social networks, so I'm personally not to inclined to use it.

But besides that, there are startup costs for a user to getting established on a new network, with all the data you've plugged into Facebook and Twitter getting stuck there. So I'm really fond of the idea of managing your social networks from a space you control like [Known][5] or WordPress, and that idea drove this redesign. It looks and feels a bit like Facebook's UI, but it's not yet a full replacement for those networks. Known and a couple other sites allow you to cross-post to Facebook, Twitter, and the like, but they'll never be a full replacement unless they can also pull in Facebook's or Twitter's stream.[^2]

That's the major feature that I'm missing from this current set up, and I'd like to build a plugin/theme combo that fills that gap for me and works. If you can view tweets and posts in WordPress and decide what to post where on a per-post basis, that would be the first step to eventually pulling your data out of your siloed social networks into a space you control.

Obviously, I'm not the only one working on something like this. David Weiner's [Little Facebook Editor][6] provides this type of native interoperability, even allowing you to update your post on Facebook when you do so in the editor, but given WordPress's ease-of-use and widespread adoption, I think there's an opportunity to reach a wider audience with a WordPress plugin. The plugins bundled with this theme do a lot of what I would like as well, but the first step for me is a TweetDeck-esque Twitter/Facebook/Tumblr/etc. client.

The idea is that you can use this tool to start saving your network posts to your own site and display that however you'd like. Once the plugin has a base in this kind network interoperability, a whole world starts to open up within this self-controlled space.

Most importantly you don't have to force people to start from scratch with a new social network to make this work. You lose the startup costs, you lose the lack of network effect, and you lose the feeling of starting over from scratch (if you don't want to). It's a big part of how Instagram became popular, allowing you to cross-post your photos to a number of networks,[^3] and I think could be a way of "back-dooring" a new, self-determined social network that could success where Diaspora* failed.

This theme is the first step towards this idea. It's currently tuned more specifically for my needs, but in the meantime, the code is available on [GitHub][7] if you'd like to take a look.

Besides the features, I'm really curious to get some feedback on the design. I'm not a designer, but I think this turned out pretty good overall. Let me know what you think.

[^1]:    
    [Custom Post Formats UI][8], [Social][9], and [Social Native Broadcasts][10]. They're all developed by [Crowdfavorite][11]. I'm a big fan of the work they've done.

[^2]:    
    The only plugin I could find that does anything like a Twitter client in WordPress is [Dashboard Twitter][12]. Unfortunately, the Dashboard in WordPress is pretty useless, so I wouldn't use that as a client.

[^3]:    
    Unfortunately, spats between the major networks after IG was bought by FB now means that IG photos don't show up in-stream on Twitter the way native photos do. Generally speaking though, photos on Twitter tend to be a bit of a mixed bag anyway. GIFs still don't show up in TweetDeck, and there isn't really a "universal" experience for pictures across all the locations you can access Twitter.

 [1]: https://github.com/crowdfavorite/wp-favepersonal
 [2]: http://foundation.zurb.com/
 [3]: http://jamesdigioia.com/new-project-wp-gistpen/
 [4]: http://jamesdigioia.com/ello-diaspora-and-the-anti-facebook-why-alternative-social-networks-cant-win/
 [5]: http://jamesdigioia.com/indieweb-advocates-launch-known-bloggers-can-social-still-control-content/
 [6]: http://littlecardeditor.com/text/
 [7]: https://github.com/mAAdhaTTah/socialwindow
 [8]: https://github.com/crowdfavorite/wp-post-formats
 [9]: https://github.com/crowdfavorite/wp-social
 [10]: https://github.com/crowdfavorite/wp-social-native-broadcasts
 [11]: http://crowdfavorite.com/
 [12]: https://wordpress.org/plugins/wordpress-dashboard-twitter/