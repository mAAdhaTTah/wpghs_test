---
ID: 6081
post_title: >
  Gatsby, Next.js, and the distance to
  your data
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/gatsby-next-js-and-the-distance-to-your-data/
published: true
post_date: 2020-11-01 18:48:13
---
<!-- wp:paragraph -->
<p>At work, we recently decided to adopt Next.js to decouple our React front-end application from our Django API, so I've been working with it extensively over the past few weeks. In parallel, I've been working to migrate this very site to Gatsby, pulling in my <a href="https://reads.jamesdigioia.com/">reading list</a> and my resume onto a new foundation, since I haven't really been working with WordPress that much lately and don't really want to make the changes here that I'm interested in. As a result, I've had an opportunity to work extensively with both tools and compare some of the benefits &amp; drawbacks, and they've got me thinking a bit about some of the struggles I've had with them.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Gatsby &amp; data fetching</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Gatsby is an extremely clever tool. Normally, when you work with APIs, you need make a bunch of requests, do a <em>bunch</em> of work to get the data into the format you need it in, make some more API requests to get whatever additional data you need, and finally send all of that into your views to be rendered by the application. Because this is process is so painful, you often just send the raw API response into the view, coupling your view to your API structure, making it really brittle and hard to reuse.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Gatsby says "to hell with all that." You still have to do some advance work to get all the data together, but all of that data is then normalized into a single, structured "back-end," queryable with GraphQL. This enables the data marshaling to be done up front, for the entire data source, in a standardized way, allowing Gatsby to generate a static site from dynamic data.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This design enables its basic selling point, but it also enables some pretty powerful features. As an example from my site, I maintain my public reading list in two places: my <a href="https://reads.jamesdigioia.com">PressForward site</a> &amp; Pocket. While I would like to combine them into a single source, for now, I'm using Gatsby to do that ahead of time. Gatsby sources all of the Pocket &amp; PF links, and I use Gatsby's <code>onCreateNode</code> function to generate normalized, dependent GraphQL nodes from both of them.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>At core, I took two separate data sources and combined this into a single, queryable data source. You can do all sources of cool things with this, connecting nodes from one data source to nodes from another. But it also reveals one of the main struggles I'm having with Gatsby.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Gatsby really takes the "everything in GraphQL" idea to the logical extreme. Images become nodes which you can then query, so they're really putting <strong>everything</strong> in there, adding a <em>lot</em> of layers between the input of your data and the output of your site.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Next.js &amp; data fetching</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By contrast, Next.js data fetching is pretty raw. You create pages in the <code>/pages</code> directory, which the app picks up and just... renders. There are a couple of data fetching functions provided which allow you to indicate when and how data should be fetched. <code>getStaticPaths</code> &amp; <code>getStaticProps</code> both indicate data that is fetched ahead-of-time, while <code>getServerSideProps</code> indicates data that is fetched at runtime.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>And that's... basically it. You return an object with a <code>props</code> key, and those props get passed to the component. It's a direct line from the API call to the transformation to the props because it all resides in this function. There aren't a lot of additional layers to plumb through to understand what's going on.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Gatsby has a complete, distinct data fetching &amp; marshaling layer that you both have to format data for correctly as well as query from correctly. Both of these things are layers of indirection you have to plumb through whenever problems arise. With Next.js, you just <code>console.log</code> your return value and you know exactly what you should see in your component.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Redux's single store</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All of this makes me think, strangely, about something else I've been using less of: Redux. There's a lot to like about Redux: It introduced the reducer pattern to a wider audience, it encourages event-driven applications, and it decouples those events from the state changes they're supposed to produce. But one of the supposed benefits has turned out to be a drawback: the single global store of state.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This was initially sold as an architectural benefit. I remember listening to Richard Feldman explain, by way of analogy, that having multiple models on the front-end made as much sense as having multiple databases on the back-end. I remember eagerly making this argument as I pushed to adopt Redux in a couple projects. And for a while, it worked fairly well.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The Elm Architecture upon which Redux is based encourages you to put <em>all</em> of your state in the store (the Model, in Elm terms). Because all of its views are functions, you literally <em>can't</em> store state anywhere else. But React has local state, which means you can store &amp; modify that data right next to where you use it to render things, and with <code>useReducer</code>, you don't even need Redux to get the reducer pattern Redux introduced.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If nothing else needs to know about that data, there's isn't a lot of reason store that data where everything can read from it. You lose the encapsulation of that data and make changing the structure of that data more difficult. Your whole application gets more brittle.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Distance to your data</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>What I'm realizing in the difficultly I'm having with Gatsby is that it's similar to the difficultly I've had with Redux: both of them put your data far away from where you actually use &amp; need it. With Gatsby, I have to go through <em>so many layers</em> to get the data from an API endpoint into a React component, from setting up and configuring the data source to querying that data with GraphQL to <strong>transforming the data into props.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I want to emphasize this last point. Most of the time, we don't write these functions and as result, our React components get coupled to the data structure of the data source. We end up with GraphQL conventions (<code>edges</code> &amp; <code>nodes</code>) scattered through out components, or we structure our Redux state to match our view's needs. In both cases, our components become brittle and more coupled to the data source. Redux in particular needs to be able to evolve its state structure independent of the view's needs.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It's really an architectural requirement that these translation functions exist, but when the distance between the creation &amp; consumption of your data is too big, by the time you get to the view layer, you're <em>done</em>. Are you really going to then write <em>another</em> function to translate the data into your view props? Hell no, you're gonna drop those props right into your views and move on with your life.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The damage caused by long distance data isn't simply the extra layers needed to shuttle data from one side of the application to the other; it's the tax on developers' mental resources expended on producing, consuming, and understanding that whole process. You hear this a <em>lot</em> about Redux; about how much boilerplate there is to write to do any little thing. But working with Gatsby makes me feel similarly: It's a lot of work to do every little thing.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Working with Next.js feels like a breath of fresh air by comparison. There's there's a couple functions to fetch data at different times, then that data goes into a page component, and... that's it. No boilerplate, no data flows, no query languages, just functions that return data and views that take that data. You're welcome to then build up those layers however you'd like, and for a lot of applications, this plus local component state is all you need. Once your application gets complicated enough to add that complexity, you can add it and design it based on your needs.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>There are benefits to the architecture Gatsby or Redux provides, but it comes with overhead, and if your application doesn't need that kind of complexity at the outset, you may be better off evolving into it. At the beginning stages of your application, aim to keep the distance your data needs to travel to a minimum, and your application will be able to evolve more flexibly.</p>
<!-- /wp:paragraph -->