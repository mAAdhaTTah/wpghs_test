---
ID: 6012
post_title: 'useEffect&#8217;s function dependency pitfall'
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/useeffects-function-dependency-pitfall/
published: true
post_date: 2020-08-17 12:50:45
---
<!-- wp:paragraph -->
<p><code><a href="https://reactjs.org/docs/hooks-effect.html">useEffect</a></code> is a powerful hook provided in React 16.8, allowing you to sync data with the outside world. One of the minor annoyances with <code>useEffect</code> is its dependency array, which indicates to <code>useEffect</code> when it should rerun. Adding a function to the dependency array can cause major problems with <code>useEffect</code>. Let's take a look at an example where adding a function <code>useEffect</code>'s dependency array causes problems for our React application.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="look-at-the-problem">A look at the problem</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The other day, I got a question from a developer on my team. She was working on some cart related functionality, and ESLint was complaining about the following code:</p>
<!-- /wp:paragraph -->

<!-- wp:intraxia/gistpen {"repoId":6043,"blobId":6044} -->
[gistpen id="6044"]
<!-- /wp:intraxia/gistpen -->

<!-- wp:paragraph -->
<p><code>refreshCart</code> is a function that does exactly what it says: Refresh the cart &amp; save the data to <a href="https://reactjs.org/docs/context.html">Context</a>. The cart is something that's used throughout the application, so we set it up as a Context for easy reuse. In this case, it was <code><a href="https://www.npmjs.com/package/eslint-plugin-react-hooks">eslint-plugin-react-hooks</a></code> warning about a missing dependency, the <code>refreshCart</code> function.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>A stroll through <code>useEffect</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>First, let's talk about what the second parameter, the dependency array, is doing.&nbsp;<code>useEffect</code>&nbsp;says "if any dependency in this array changes, rerun the effect". Think about this as a "synchronization mechanism", keeping your Component's data &amp; some external side effect in sync with each other.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We have a great example of this in our app: On one page, we want to load a list of potential plans based on what the user has entered into the form. As the user changes the values in the form, we make a new API request to fetch those plans with <code>useEffect</code>, keeping the latest request in sync with form data and cleaning up the previous request as we go.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So there are consequences to leaving values out of the dependency array. It can cause issues where the effect doesn't run when you expect it to, or is running selectively when some data changes but needs to run when all of the data changes, or functions are running with stale data. The ESLint rules call this out so you can handle it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>The problem with <code>refreshCart</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The reason this is annoying in&nbsp;<em>this</em>&nbsp;case is&nbsp;<code>refreshCart</code>&nbsp;is defined within the scope of the component, so it's&nbsp;<em>created fresh</em>&nbsp;on every render. If you were to just stuff it into the dependency array as it tells you, you'd get an infinite loop. On initial mount, the effect would call the function, which fetch the cart &amp; saves the data, triggering a render. When it does, it would see&nbsp;<code>refreshCart</code><em> as a new reference</em>. <code>useEffect</code>&nbsp;goes "oh hey, my dependency changed, I should rerun the effect", starting the whole cycle over again.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Note that you only have this problem with functions created with in the component function. If you can hoist the function out of the component scope, ESLint will stop complaining because <code>useEffect</code> will see the same function reference on every render.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We could do that in this case because <code>refreshCart</code> saves the data to a <code>useState</code> hook. This state is eventually passed into the Cart context mentioned earlier, so we need <code>refreshCart</code> to be within the component scope so it has access to <code>setCartContext</code>, the setter returned by <code>useState</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>What we can do about it</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are a couple solutions to this:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>Wrap&nbsp;<code>refreshCart</code>&nbsp;in&nbsp;<code>useCallback</code>, a specialized version of&nbsp;<code>useMemo</code>. Similar to <code>useEffect</code>, <code>useMemo</code> calls the callback provided whenever any of the dependencies changes, but instead of performing a side effect, it memoizes the return value. This is helpful for minimizing the number of times you recalculate a value as well as maintaining references between renders. In the latter case, <code>useCallback</code> provides a specialized version of <code>useMemo</code> for creating callbacks, where the function reference is reused if the dependencies haven't changed.</li><li>Assign <code>refreshCart</code> to a ref created with <code>useRef</code>. ESLint doesn't complain about ref usage in effects because they're intentionally designed for mutable data. If you do this, you can create a singleton that you assign on first render and never touch again. If you mutate it further, it could cause issues in Concurrent Mode in the future. You can use this if you know for certain there are no dependencies or they will never change.</li><li>Tell eslint to shut up with&nbsp;<code>eslint-disable-next-line</code>. Generally, eslint yells at you about these things for good reasons, so this is really a last resort if neither of the above solutions work for various reason. If you do go this route, you should always leave a comment as to why.</li></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Solution 1</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If we assume our <code>refreshCart</code> function looks something like this:</p>
<!-- /wp:paragraph -->

<!-- wp:intraxia/gistpen {"repoId":6043,"blobId":6048} -->
[gistpen id="6048"]
<!-- /wp:intraxia/gistpen -->

<!-- wp:paragraph -->
<p>Then we can wrap the function in <code>useCallback</code> like this:</p>
<!-- /wp:paragraph -->

<!-- wp:intraxia/gistpen {"repoId":6043,"blobId":6052} -->
[gistpen id="6052"]
<!-- /wp:intraxia/gistpen -->

<!-- wp:paragraph -->
<p>In this case, which is similar to our real application, we can leave out <code>setCart</code> as a dependency. ESLint knows that <code>setCart</code> will always be the same reference because <code>useState</code> (as well as <code>useReducer</code>) returns a stable function reference as the second value in the array. If your function has additional dependencies, ESLint can fix that for you automatically. Wrap your <code>refreshCart</code> equivalent in <code>useCallback</code> with an empty dependency array and run <code>eslint --fix</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Solution 2</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In the above example, we know for sure that everything we're using in the function is a stable reference and won't change over time. If you're positive this is the case, and you know that won't eventually change, you can instead assign the function to a ref with <code>useRef</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:intraxia/gistpen {"repoId":6043,"blobId":6055} -->
[gistpen id="6055"]
<!-- /wp:intraxia/gistpen -->

<!-- wp:paragraph -->
<p>Couple of things to be aware of here:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>We only create the function once, the first time the reference is created. On future renders, <code>refreshCart.current</code> will point to the same function reference as the initial render, so the <code>if</code> block won't run. This <em>will</em> cause a problem if <code>refreshCart</code> references values outside of its scope that change over time. This is know as the "stale closure problem". One way you can avoid this problem is using the callback form of <code>setState</code>, which eliminates the dependency on the state value itself.</li><li>You <em>must</em> use the ref directly in <code>useEffect</code> in order to keep ESLint from complaining. Because ESLint knows that refs are designed for mutable data, it knows not to include them in <code>useEffect</code>'s dependency array.</li></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The "stale closure problem" is a big potential issue here, so it's worth emphasizing: You <em>will</em> end up with unexpected behaviors if you rely on <code>useState</code> values, props, or other changing values within your <code>refreshCart</code>. This is because the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures">closure captures the values in its scope</a>. Because the function is never recreated, it never captures the updated values in its scope. Hence "stale closure problem."</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Solution 3</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As a last resort, and if you know exactly what you're doing and what the consequences are, you can always tell ESLint to leave you alone. If you choose to go this route, be sure to leave a comment explaining <em>why</em>, so future you knows what the thinking was. Additionally, you should disable the specific rule that ESLint is complaining about, so if that part of the code has additional problems, ESLint will still complain about them.</p>
<!-- /wp:paragraph -->

<!-- wp:intraxia/gistpen {"repoId":6043,"blobId":6058} -->
[gistpen id="6058"]
<!-- /wp:intraxia/gistpen -->

<!-- wp:paragraph -->
<p>Now, if you come back to this, you know why this rule was disabled, what the purpose is, and whether that logic still applies. If you're making changes to <code>refreshCart</code>, you may want to refactor this code into one of the above solutions.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Conclusion</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The ESLint plugin is incredibly helpful for catching situations like this, and with these tools in your tool belt, you can choose the best answer for your situation. I would also argue this is the preference order for these 3 solutions; do <code>useCallback</code> first. There are good reason to use the other two, and you should have them available to you, but all of these are dependent on <em>how the code will change</em>. The less likely it is to change, the further down the list you can go. However, <code>useCallback</code> (&amp; <code>useMemo</code>!) leans <em>into</em> the way hooks are designed, with the dependency array linking everything all the way back. That's why it's the best solution for the common case and what you should reach for first.</p>
<!-- /wp:paragraph -->