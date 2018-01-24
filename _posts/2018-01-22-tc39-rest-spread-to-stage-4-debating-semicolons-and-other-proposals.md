---
ID: 5541
post_title: 'TC39: Rest/Spread to Stage 4, debating semicolons, and other proposals'
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/tc39-rest-spread-to-stage-4-debating-semicolons-and-other-proposals/
published: true
post_date: 2018-01-22 16:25:09
---
_Update: All of the Stage 3 proposals below have advanced to Stage 4! 🎉_

This week, the TC39, the standards body behind the JavaScript language, will be meeting at Google this week, January 23-25, for their first meeting of 2018. They'll be discussing several proposals to add new features to ECMAScript, the JavaScript standard. Their full agenda can be found [here][agenda], but I wanted to take a quick look at some of the important proposals they'll be discussing.

This first one is the most important to me, and that's [Rest/Spread properties][rest-spread], which you may be familiar with from your usage of React & JSX. In fact, this proposal originated there as a method for easily passing properties down to child components. They'll be discussing advancing it to Stage 4, which would make it officially part of the language. All of the entrance criteria [seem to be met][criteria], it's clearly popular in the JavaScript community (basically any tutorial using Redux uses it), and it's already supported in [V8][v8], which means Chrome and Node both support the syntax. All that's left is for the committee to accept it. I expect this proposal to land in the language at this meeting.

The other important proposals on the docket are the a couple of proposals to expand the capabilities of JavaScript's built-in Regex object. The first is [lookbehind in Regex][lookbehind], which is a feature commonly found in other Regular Expression engines. I know at least for those of us working on [PrismJS][prism], this should be a really useful feature for the highlighting engine. The other is [Unicode property escapes][escapes], which I honestly don't know much about. Both of these are looking to advance to Stage 4 as well.

Additionally, both Async Iteration & `Promise.prototype.finally` are set to be discussed for Stage 4. [Async Iteration][async-iter] is an expansion on the iterator protocol (think `for..of` & `Symbol.iterator`) for the asynchronous resolution of values. This actually could be really interesting / useful for interoperating with Observables, as it provides a pull-based method forasync values, so Async Iteration could provide a useful "seam" between push-based Observables and a pull-based consumer.

[`Promise.prototype.finally`][finally] is used to provide a callback after a Promise has resolved. It's not exactly the same as `try / catch / finally` when using `async / await`, as the `finally` block in that case can contain more `await`'d promises. A lot of Promise libraries have this method, commonly used for cleaning up resources like database connections or turning off a spinner after the request complete. This doesn't change your usage of `async / await` itself, as it's a prototype method on the Promise object, and they don't really function the same either.

Finally, the big one is a conversation around TC39's latest [proposed guidance in favor of using semicolons][pro-semi] instead of relying on ASI, which generated a lot of controversy, most significantly from JavaScript creator [Brendan Eich][dissent]. To be clear, the recommendation was non-normative, meaning it won't have any impact on actual development of the language. It's a recommendation from the committee that the development of new features in the JavaScript language is likely to introduce new Automatic Semicolon Insertion (ASI) hazards, e.g. areas where relying on ASI could have unexpected results. It is **not** a statement from the committee to say they will no longer care about ASI, or that ASI is now deprecated; it's a recognition by the committee that the no-semi style is likely to become more hazardous over time, and that the best way to avoid said hazards is to... use semicolons.

Obviously, this caused a lot of consternation in the JavaScript community, especially as one of the most popular linting presets, [standard][standard], recommends the no-semi style. Eich's dissent, as the creator of JavaScript, obviously carries a lot of weight, and his argument is essentially that the guidance will have zero real-world impact, as the no-semi style is very widespread, and the TC39 shouldn't discourage this style but should instead recommend using tools to avoid ASI hazards in new features. He's also concerned the guidance will make it more likely the committee will be comfortable introducing ASI hazards. In fact, we're currently contemplating [a potential ASI hazard][asi-hazard] for the pipelining proposal, so it's entirely possible new hazards will be introduced in future proposals / features.

Regardless, the discussion will definitely be interesting for anyone who's attending, and I'm curious if TC39 is going merge the guidance or not. My suspicion is no; the decision seems controversial for little to no material benefit for the committee.

The meeting run Tuesday - Thursday of this week, so keep an eye out for any announcements from the committee as they finalize the latest version of ECMAScript.

  [agenda]: https://github.com/tc39/agendas/blob/master/2018/01.md
  [rest-spread]: https://github.com/tc39/proposal-object-rest-spread
  [criteria]: https://github.com/tc39/proposal-object-rest-spread/issues/32
  [v8]: https://developers.google.com/web/updates/2017/06/object-rest-spread
  [lookbehind]: https://github.com/tc39/proposal-regexp-lookbehind
  [prism]: http://prismjs.com
  [escapes]: https://github.com/tc39/proposal-regexp-unicode-property-escapes
  [async-iter]: https://github.com/tc39/proposal-async-iteration
  [finally]: https://github.com/tc39/proposal-promise-finally
  [pro-semi]: https://github.com/tc39/ecma262/pull/1062
  [dissent]: https://twitter.com/BrendanEich/status/951554266535141377
  [standard]: https://standardjs.com/
  [asi-hazard]: https://github.com/tc39/proposal-pipeline-operator/issues/83#issuecomment-359101924