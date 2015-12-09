---
post_title: 'Fat Arrow Functions, One-Line Callbacks, and Composing Promises'
layout: post
published: true
---
I'm not really a fan of arrow functions in ES2015 for the same reason I don't like the introduction of `class`: JavaScript is a prototypal language, so attempting to cram class-based inheritance into it goes against the whole structure of the language. The prototype chain the closure scoping makes for a number of interesting patterns for solving problems and structuring your code. [Eric Elliott](https://vimeo.com/69255635) has written a number of [really interesting articles](https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a) about object composition that have been really though provoking.

Fat arrow functions are primarily used to bind a function's `this` to its lexical scope. That's not [*all*](https://github.com/getify/You-Dont-Know-JS/issues/513) it does; they also bind their `arguments` and `super`. There's a [great explanation](http://blog.getify.com/arrow-this/) about how they fully work, but they're very commonly used to bind `this` to the scope of the arrow function. That's not what we need; we need to use scoping more effectively so binding `this` isn't necessary, and play into the strengths of the language.

I am, however, a huge fan of Promises, and I've been working on a project recently that makes heavy use of them, and not only does it make handling asynchronous code a breeze, it's much easier to build concurrency into the application. With the Bluebird library, composing Promise chains and setting up dependencies for a set of asynchronous actions is *fun*. It is during the composition of these chains that fat arrow functions make for really elegant syntax.

When you're composing together a chain of Promises to retrieve a particular value, you'll often find yourself doing these one-off transformations:

[gistpen id=4753]

This is a really simple example, but something that comes up all the time when composing Promise chains: You need to take the value from a previous function and do some small manipulation to it to get the value you're looking for.

With fat-arrow functions, the above 3 lines become a single line:

[gistpen id=4764]

Fat arrow functions, when written as a one-liner, automatically returns the value, so any function that works as simply as this does are vastly improved by writing them this way. This comes up all the time, especially when using libraries that depend on Promise chains. You'll often find yourself calling asynchronous methods on asynchronously returned objects, so chaining together a set of Promise methods become really clean:

[gistpen id=4769]

If you have 3 or 4 of these steps, you can see how writing these becomes an absolutely joy.

The other thing writing fat arrow functions do is, when written on one line like this, they **always return a value.** One of the most annoying bugs to solve is when a Promise chain is falling down because you forgot to return a value or a Promise somewhere in the chain. Because one-line fat arrow function always return a value, they protect you from making this mistake, thus saving you from a lot of time debugging stupid problems.

I've started rewriting a lot of my callbacks so far into one-line fat arrows, and it's been lovely so far. They're really nice to both read and write, and I highly recommend them for this particular use-case.