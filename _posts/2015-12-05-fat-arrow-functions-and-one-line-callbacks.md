---
post_title: 'Fat Arrow Functions and One-Line Callbacks'
layout: post
published: true
---
I'm not really a fan of arrow functions in ES2015 for the same reason I don't like the introduction of `class`: JavaScript is a prototypal language, so attempting to cram class-based inheritance into it goes against the whole structure of the language. The prototype chain the closure scoping makes for a number of interesting patterns for solving problems and structuring your code. [Eric Elliott](https://vimeo.com/69255635) has written a number of [really interesting articles](https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a) about object composition that have been really though provoking.

I am, however, a huge fan of Promises, and I've been working on a project recently that makes heavy use of them, and not only does it make handling asynchronous code a breeze, it's much easier to build concurrency into the application. With the Bluebird library, composing Promise chains and setting up dependencies for a set of asynchronous actions is *fun*. It is during the composition of these chains that fat arrow functions make for really elegant syntax.

I've been using nodegit, a library with Promises already integrated, to interact with git repositories. In this case, I needed to open the repository and grab its index in order to do another action. These needs need to take place sequentially, because you pull the index out of the repository you checked out.

```js
return Bluebird.resolve(git.Repository.open(repo)).then(function (repo) {
    return Bluebird.resolve(repo.index()).then((index) => [repo, index]);
})
    .spread(function (repo, index) {
        // Act on the repo and index
        return value;
    });
```

The `spread` function turns the array into the parameters for the function passed into it. In order to compose those parameters, I need to return an array formatted that way. It's a simple function, but one that would normally take 3 lines:

```js
return Bluebird.resolve(repo.index()).then(function(index) {
     return [repo, index];
});
```

becomes one when written as a fat arrow function:

```js
return Bluebird.resolve(repo.index()).then((index) => [repo, index]);
```

In fat arrow functions, when written as a one-liner, returns the value, so any function that works as simply as this does are vastly improved by writing them this way. I don't know how common this particular use case is, but I suspect there are plenty of one-liner callbacks that would look good like this.