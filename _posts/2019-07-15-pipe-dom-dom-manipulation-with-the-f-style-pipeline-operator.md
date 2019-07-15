---
ID: 5882
post_title: 'pipe-dom: DOM manipulation with the F#-style pipeline operator'
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/pipe-dom-dom-manipulation-with-the-f-style-pipeline-operator/
published: true
post_date: 2019-07-15 14:06:18
---
Last week, Babel released version [7.5.0][babel-75], which included our implementation of the [F#-style pipeline operator][fsharp-pipe]. If you're not already aware, TC39 is exploring the potential of a pipeline operator in JavaScript. You can learn more about what's going on with the operator [here][pipe-status]. At this point, we've got all 3 proposals in Babel, so the next step is to start getting feedback on them. To help with that, I've put together a small DOM manipulation library, [`pipe-dom`][pipe-dom], for the newly-released F#-style pipeline operator.

When new developers start learning JavaScript, they often start with jQuery. One of jQuery's defining characteristics is its fluent API, which allows you to write clear, concise code when making a series of DOM modifications. The major downside to this API style is all of these methods need to be attached to the jQuery object, which means the entire library needs to be loaded to be usable. Minified and gzipped, jQuery is ~30KBs, which is a lot to include if you're just trying to toggle classes.

With the introduction of modules to JavaScript, bundlers are able to analyze what's used in a project and remove unused functions, a process called [tree-shaking][tree-shake]. `pipe-dom` takes jQuery's fluent API and combines it with the pipeline operator, allowing users to import the exact DOM methods they want and let bundlers remove the rest. Here's what that might look like:

[gistpen id="5890"]

With this, your bundler can include just the code for `query`, `addClass`, `append` and `on`, and discard the rest of the library.

I've included a small API initially to get the idea out there, but I'm very interested in expanding it, so please [open an issue][new-issue] and suggest ideas! I'm very open to expanding the API and evolving the library along with pipeline operator best practices.

[Check it out][pipe-dom] and [let me know what you think][tweet-at-me]!

[babel-75]: https://babeljs.io/blog/2019/07/03/7.5.0
[fsharp-pipe]: https://github.com/valtech-nyc/proposal-fsharp-pipelines
[pipe-status]: https://babeljs.io/blog/2018/07/19/whats-happening-with-the-pipeline-proposal
[tree-shake]: https://webpack.js.org/guides/tree-shaking/
[pipe-dom]: https://github.com/mAAdhaTTah/pipe-dom
[new-issue]: https://github.com/mAAdhaTTah/pipe-dom/issues/new
[tweet-at-me]: https://twitter.com/JamesDiGioia