---
ID: 5874
post_title: 'React Testing Tip: Reduce test duplication with a component render function'
author: James DiGioia
post_excerpt: ""
layout: post
permalink: >
  https://jamesdigioia.com/react-testing-tip-reduce-test-duplication-with-a-component-render-function/
published: true
post_date: 2020-04-27 08:56:00
---
Whenever you write new tests for your React components, you'll probably find your tests reusing the same interactions as tests you've already written. In multiple tests, you'll click a button or change an input and assert that the React component you're testing updates as expected. This comes up often enough when testing that I like to start my tests by making those interactions reusable. Let's look at how we can do that.

## Basic Tests

For testing React component, I use [Jest][jest] & [@testing-library/react][rtl]. For this example, we're going to be testing this basic form:

[gistpen id="5965"]

We need to write at least 2 tests for this. First, we'll attempt to submit the form immediately and check that it fails because the form does not have a value yet. Then we'll test changing the value in the form and then submit it, and it should succeed with the value. Let's take a look:

[gistpen id="5968"]

In both tests, we create a mock function with `jest.fn()` to provide to the rendered `Form` component. In the first test, we assert that this mock function has not been called, as we don't want an empty value sent to the `submit` function. In the second test, we first change the value in the form field then submit the form. This time, we succeed, as we have a valid value in the form.

## Writing a render function

These are small tests, but we already see some duplication. The `button` is queried with the same code twice, the `change` call is verbose, and `render` is the same in both tests. All of this would be more readable if we had reusable functions for all of it. Let's create a `renderForm` function that will reduce this duplication:

[gistpen id="5971"]

All of the repeated logic is bundled up in named functions and we can use it like this:

[gistpen id="5974"]

These tests provide a much clearer explanation of what is supposed to happen, and we could easily add a third test if we wanted to confirm changing back to an empty string still results in `submit` not being called:

[gistpen id="5977"]

Now we're really starting to see the benefits of this render function! Less code duplication, as we can reuse these `fire` functions, but more importantly, if the way we need to query the element changes, we only have to change it one place. If we decided to change the label on the `input` field (which we should, because "Type" isn't very descriptive), we only have to change it in the `renderForm` function.

## Introducing `react-testing-kit`

Because this pattern has been so useful to me, I created a package to make creating them easier called [react-testing-kit][rtk]. Let's take a look at how we can simplify this code with it:

[gistpen id="5980"]

RTK passes `elements` created in that function to the `fire` function and returns the result of all of those functions when you create a new instance. Let's update the last test to use this function:

[gistpen id="5983"]

Very similar but less boilerplate.

## Conclusion

We can simplify all of our tests with [react-testing-kit][rtk]. If this looks like it would improve your code, [check out the project][rtl] and let me know what you think!

[jest]: https://jestjs.io/
[rtl]: https://testing-library.com/docs/react-testing-library/intro
[rtk]: https://github.com/mAAdhaTTah/react-testing-kit