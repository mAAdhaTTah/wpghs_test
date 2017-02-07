---
ID: 5263
post_title: >
  Using Observables to Control Render
  Scheduling
author: James DiGioia
post_date: 2016-11-07 12:17:08
post_excerpt: ""
layout: post
permalink: >
  http://jamesdigioia.com/using-observables-to-control-render-scheduling/
published: true
---
When I talk to other developers about Observables, it's often hard to explain their benefits because on the surface, they just look like glorified event emitters, but it's how they _compose_ that make handling dependencies between async events such a breeze. While working on the code snippet editor in [WP-Gistpen][gistpen], I came across an example that shows how easily Observables handle async behavior.

So here's the setup: building a snippet editor on top of [PrismJS][prism] and borrowing some code from [Dabblet][dabblet], both from [Lea Verou][lea], and using [Redux][redux] as an Observable of states, we need to rerender the editor as the text changes, keeping the syntax highlighting up to date. We keep the position of the cursor and the value of the editor in the Store, allowing the reducer to be responsible for the complex logic that handling special text editor keystrokes, like enter, tab, etc.

The issue is rendering the editor requires us to track & reset the cursor, as we have to reset the text with the latest from the store and rehighlight it. If we do that while the user is typing, we have the potential to interrupt her, so we need to delay the render until the user is no longer typing. The render itself is scheduled in a `requestAnimationFrame`, so if the user types before the next frame, we need to cancel the render request and wait until the user stops typing again. Otherwise, when the frame renders, the cursor will jump back to the position it was at when the render was scheduled.

So this is the problem we're trying to solve.

In [`brookjs`][brook], a component is just a function that takes a DOM element & a stream of `props$` and returns a stream of (usually [Flux Standard][fsa]) Actions from the element. Since we're given an Observable of state, we're able to get pretty fine control over exactly when the editor renders.

I'm using [Kefir][kefir], but the same concepts apply any other Observable implementation. Here's how I solved this:

[gistpen id=5272]

This problem is obviously solvable in a stateful way, with the view being responsible for holding onto a reference to the render loop, scheduling it in one event callback if a render isn't scheduled and cancelling the render on the other if it is. We're not talking about a huge component here, so there's no reason to think managing this state would be difficult.

But as this component grows in complexity (and it will), colocating the temporal dependencies makes it very easy to reason about how the component changes over time. In the above example, how keyup and keydown events interact with the render cycle is explicit, rather than bouncing between callbacks or methods to see how animations are scheduled and cancelled. There's zero chance of accidental cancellations or double renders.

Side effects, like updating the DOM, are wrapped in a stream, so they can be handled asynchonrously and cancelled, if needed. Because an Observable comes with its own cleanup code (the function returned at the end of `stream`'s callback), when the active Observable is switched, the animation frame request for the previous Observable is cancelled, ensuring the user isn't interrupted as she types. The `props$` stream has control over when the render happens, and `flatMapLatest` ensures only the newest Observable is being observed at a time, so as the `props$` emits new state, previous renders are also cancelled.

`brookjs` provides some structure around this paradigm, but underneath, this is all that's happening, and it provides some elegant solutions to thorny async problems. The canonical example is the autocomplete box, sending an AJAX request as a user types and cancelling the previous request. This is explained by Jafar Husain in [this great talk][async-js-at-netflix].

Even after writing this article, the editor rendering continued to get more complicated, and handling it with Observables allowed me to focus on how particular events interacted with the render cycle, without worrying about how to manage what was actually _happening_ at a given time. Let me know if you think the [current implementation][impl] is easy to understand.

  [gistpen]: https://github.com/mAAdhaTTah/WP-Gistpen
  [prism]: http://prismjs.com/
  [dabblet]: http://dabblet.com/
  [lea]: http://lea.verou.me/
  [redux]: http://redux.js.org/
  [brook]: https://valtech-nyc.github.io/brookjs/
  [fsa]: https://github.com/acdlite/flux-standard-action
  [kefir]: http://rpominov.github.io/kefir/
  [async-js-at-netflix]: https://www.youtube.com/watch?v=XE692Clb5LU
  [impl]: https://github.com/mAAdhaTTah/WP-Gistpen/blob/85a392c0e20da87610c0677e25c24ef00cba82b6/client/editor/instance/onMount.js#L196-L273