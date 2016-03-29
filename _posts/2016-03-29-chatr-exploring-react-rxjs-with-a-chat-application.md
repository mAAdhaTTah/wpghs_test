---
ID: 4875
post_title: 'Chatr: Exploring React &#038; RxJS with a Chat Application'
author: James DiGioia
post_date: 2016-03-29 13:37:22
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/chatr-exploring-react-rxjs-with-a-chat-application/
published: true
---
[React.js][1] has pretty well skyrocketed through the JavaScript community since its initial announcement, and with its reactive approach to UI, there’s been a growing interest in functional programming in the JavaScript community as a result. Because JavaScript allows you to pass functions around like objects, it makes it very easy to apply functional concepts to the language. Like much of the community, I’ve also been [reading][2] [quite][3] [a bit][4] about [functional programming][5], but I haven’t had the opportunity to build a full-fledged application on these ideas. I’ve played around with [RxJS][6] & React[^1] a little bit, but I’m not entirely sure what a full-stack architecture might look like (yet). I want to explore the possibilities by building a real-time chat application (with the uninspiring name Chatr) on Node.js with RxJS, React, & [Ramda][7], applying functional programming to a system that can easily be modeled as a stream of messages/events. With RxJS, I should be able pipe messages around the application, including into React to update the UI on the client side, as well as to and from a data store like [Redis][8] or a messaging queue like [RabbbitMQ][9] on the server side, with the data streaming between the server & client over a Socket.io stream. This architecture could also be inspired by [Flux][10], ensuring all data flows into a central “store stream,” which then pipes out the messages to wherever it needs to go. This primary data store stream probably could be reused on both the client & server side, which allows us to introduce some isomorphism in both our data handling & UI. My plan is to build this in public and write about it, and see what some of the advantages and drawbacks of this approach for building web applications. You can follow along with [the repo on GitHub][11], or keep up with the [thread on this website][12]. Before we can get started doing anything interesting, we’ve got to get some boilerplate going. Since we want to try out some isomorphic techniques, we’re going to need to, at a minimum, get the JSX compiled on the server side. [`babel`][13] is the default standard for compiling JSX, and they have a very clear [example][14] for getting it running on the server. Since we’re already using it, we’re also going to bring in ES6 compilation and use that for both the server- and client-side code. Following along with the example, we’re going to add `nodemon`, `babel-cli`, and the two babel presets we’re going to use, `babel-preset-es2015` and `babel-preset-react`. Since we’re going to be sharing the `babel` configuration between the server and client, we need a `.babelrc` file, which `babel` uses to register presets and plugins: http://jamesdigioia.com/gistpens/chatr-boilerplate/babelrc-2/ Finally, add `nodemon server.js --exec babel-node` to the `package.json`’s scripts key as `"start"`, so we can run `npm start` to run the server. `babel-node` is shipped with the `babel-cli` package we installed, and it ensures our server-side code is compiled on-the-fly by `babel` and then run in `node`, with `nodemon` recompiling and restarting the server whenever our code changes. We’re going to use [express.js][15] to handle routing and serve our static assets. Let’s create the `server.js` file and get some simple routes going: http://jamesdigioia.com/gistpens/chatr-boilerplate/server-js-2/ Pretty basic express server; run `npm start` in your terminal and then go to [localhost:3000][16]. You should see a big “Hello World!”. Got it? Good! Let’s get some script and style compilation going. We’ll start with the scripts. [`webpack`][17] and [`browserify`][18] are both solid options for compiling scripts using `babel`. I’ve used `browserify` a lot more than I’ve used `webpack`, so I’m going to use `webpack` for this project. Fortunately, the configuration isn’t that complicated for a simple setup like this: http://jamesdigioia.com/gistpens/chatr-boilerplate/webpack-config-js-2/ We’re not going to worry about getting any of the really complicated features setup, like hot reloading or dev servers or anything like that. Instead, we’re going start with to create a simple script that uses some ES6 to ensure that we’re compiling our scripts correctly. http://jamesdigioia.com/gistpens/chatr-boilerplate/client-js-2/ If you’ve `npm install`ed `webpack`, you can just run `webpack` in your terminal and it should spit out a `main.min.js` file in your `public` folder. Pull that up in your [browser][19] and you should see the compiled file. On the style side, we can compile our `styles.scss` file with `node-sass`. I’m going to be including [Bourbon][20], [Neat][21], and [Bitters][22] for this project, as I like their mixin-only approach for its flexibility and control. Here’s the very basic `styles.scss` file: http://jamesdigioia.com/gistpens/chatr-boilerplate/styles-scss-2/ I’d love to get this setup with [Eyeglass][23] as well, but we’ll start with this. Install `node-sass` and run `node-sass styles.scss public/styles.css`. We [should see][24] the CSS file rendered, compressed and compiled correctly, with a long sourcemap appended at the end. Finally, we’re going to convert the root route to render a template for us instead of using a simple string. In this case, we’re going to register [Handlebars][25] using [`express-handlebars`][26]: http://jamesdigioia.com/gistpens/chatr-boilerplate/handlebars-2/ which we’re going to use to render a very simple template: http://jamesdigioia.com/gistpens/chatr-boilerplate/main-hbs-2/ This is set up this way in preparation for implementing is server-side rendering! The `app` variable in the template context will become the rendered HTML string from React, and the `state` variable will be the state object that produced the given HTML. Then on the client side, we’ll pull in current state, render the React components on the current state on the page, and bootstrap the application. This is how the [Redux docs][27] suggest doing it, and since they know what they’re doing, we’re going to follow their lead. But we’re going to wire that up in the next tutorial. For now, you should have a simple page page, rendering “Hello World!”, with a CSS file and a script that outputs `3` to the console. Next, we’re going to write our first React component, render it on the server, and bootstrap our application. 
[^1]:    In the next version of WP-Gistpen, the settings page is built with React & RxJS.
    <a href="#fnref:1" rev="footnote">↩</a>

 [1]: https://facebook.github.io/react/
 [2]: https://medium.com/@chetcorcos/functional-programming-for-javascript-people-1915d8775504#.2bmh3geet
 [3]: http://fr.umio.us/favoring-curry/
 [4]: http://fr.umio.us/why-ramda/
 [5]: https://github.com/timoxley/functional-javascript-workshop
 [6]: https://github.com/Reactive-Extensions/RxJS
 [7]: http://ramdajs.com/0.19.1/index.html
 [8]: http://redis.io/
 [9]: https://www.rabbitmq.com/
 [10]: https://facebook.github.io/flux/
 [11]: https://github.com/mAAdhaTTah/chatr
 [12]: http://jamesdigioia.com/thread/rxjs-react-w-chatr/
 [13]: https://babeljs.io/
 [14]: https://github.com/babel/example-node-server
 [15]: http://expressjs.com/
 [16]: http://localhost:3000/
 [17]: https://webpack.github.io/
 [18]: http://browserify.org/
 [19]: http://localhost:3000/main.min.js
 [20]: http://bourbon.io/
 [21]: http://neat.bourbon.io/
 [22]: http://bitters.bourbon.io/
 [23]: http://eyeglass.rocks/
 [24]: http://localhost:3000/styles.css
 [25]: http://handlebarsjs.com/
 [26]: https://github.com/ericf/express-handlebars
 [27]: https://github.com/reactjs/redux/blob/master/docs/recipes/ServerRendering.md