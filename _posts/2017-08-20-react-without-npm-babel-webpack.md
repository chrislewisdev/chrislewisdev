---
layout: post
title: React without npm, Babel, or webpack
description: A look at how to get started using React without all the requirements of package managers and bundlers.
---

When I start learning a new tool or framework, I like to try and keep it as isolated from other concepts as possible, so I can focus just on that one thing and nothing else. It avoids frustration with auxiliary tooling and helps me make sure I understand exactly what is happening in any given example.

In the case of learning React, I wanted to have a full working example in a plain HTML/Javascript file that I can simply open in any browser- no development webserver or anything involved.

Sounds simple enough, right? Well, it *is *once you know how, but the React documentation makes it a little difficult to tell how best to do this. To quote:
> While React [can be used without a build pipeline](https://facebook.github.io/react/docs/react-without-es6.html), we recommend setting it up so you can be more productive. A modern build pipeline typically consists of:

> A **package manager**, such as [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/). It lets you take advantage of a vast ecosystem of third-party packages, and easily install or update them.

> A **bundler**, such as [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/). It lets you write modular code and bundle it together into small packages to optimize load time.

> A **compiler** such as [Babel](http://babeljs.io/). It lets you write modern JavaScript code that still works in older browsers.

Now donÔÇÖt get me wrong, those are all good tools to use, but thatÔÇÖs a lot of things I donÔÇÖt want for just a simple sample file! My aim with this post is to collate all the necessary information to avoid all those things altogether.

LetÔÇÖs look at how we can avoid the need for all of these things by converting [this CodePen sample](https://codepen.io/anon/pen/brvpjG?editors=0010) into something we can run in a plain HTML file:

<iframe src="https://medium.com/media/7a635b3686e08618367032d63118bd20" frameborder=0></iframe>

*Note: I wonÔÇÖt be explaining how React actually works in this post, just how to translate from most React samples into easily runnable code. If you want to know how this sample actually works, consult the [Quick Start Guide](https://facebook.github.io/react/docs/hello-world.html) for React.*

## Avoiding npm

In order to avoid setting up npm for the sake of a *single file*, we can reference a CDN for the two main React libraries, react and react-dom:

<iframe src="https://medium.com/media/7dc48e1d467424f36e2fbb3f9f4778f9" frameborder=0></iframe>

Alternatively, you can manually save those two JS files into your workspace and reference them locally. (itÔÇÖs no big concern for a sample file, but I did on one occasion find the CDN had been updated with a broken file, which broke my sample project)

## Avoiding webpack

Since all our work will be in a single file, we wonÔÇÖt run into any need for webpack in this sample as is. Even if your samples expand and you want to break stuff into several files, you can just include them all in index.html with script tags- thereÔÇÖs honestly no need to introduce a build system for only a handful of files!

## Avoiding Babel

This is where weÔÇÖll actually have to make some modifications to the JS code to get it running. Most React samples use JSX, aka the bits that mix in HTML syntax inside JavaScript:

<iframe src="https://medium.com/media/ba06e4d3f8a8209dfdf0ed64b99e036b" frameborder=0></iframe>

If you try and run anything like that locally, youÔÇÖll just get a boring old syntax error message in your console.

However, that JSX code is actually just syntactic sugar for the following:

<iframe src="https://medium.com/media/1e550181c5914c7705cb54c8f6691000" frameborder=0></iframe>

Any JSX block can be converted into a call to React.createElement with three arguments:

* The type of element to create. Since weÔÇÖre just creating a standard H1 element, as opposed to a custom React component, we specify it with a plain old string.

* A properties object which specifies any property values to be set on that element. Since weÔÇÖre only creating a plain H1 element, null will suffice, but we could use this to specify a CSS class on it, for example.

* Any child elements to place in it. For plaintext contents like weÔÇÖre using here, a string is all you need, but IÔÇÖll give an example at the bottom where we pass in additional React components.

And thatÔÇÖs it! So, as for the other JSX block in our code:

<iframe src="https://medium.com/media/9c8ba7cf0f480139b14b458ef4461a2b" frameborder=0></iframe>

This can just be expressed as:

<iframe src="https://medium.com/media/d4b735ccb5dba7704f2a0e6ae8c03b1f" frameborder=0></iframe>

This time we are actually providing some properties to the element, and since we donÔÇÖt need to give it any child elements, weÔÇÖre omitting the third parameter altogether.

## The End Result

ThatÔÇÖs honestly all it takes to get React working without all the guff of package managers and build systems. All together, it looks like this:

<iframe src="https://medium.com/media/8e68caf8e069a9e5ffa44b09bb9e7c9a" frameborder=0></iframe>

Give that a shot on your local machine and it should work right out of the box!

That should be all the setup you need to start toying with React examples whilst knowing thereÔÇÖs no unknown magic occurring in the background (at least, outside of React itself). Start working through the [Quick Start Guide](https://facebook.github.io/react/docs/hello-world.html), and enjoy!

## Additional Notes

### Passing multiple children to React.createElement

As I mentioned earlier, you can provide as many children to an element created with React.createElement as you like. Suppose instead of one Greeting you wanted to nest multiple Greetings within a div tag:

<iframe src="https://medium.com/media/fe03a9f0b30283a8f4a92f0e4ebdb9f0" frameborder=0></iframe>

Simple! And honestly not too ugly if you space out the function calls sensibly.

### I want all the extra tooling but donÔÇÖt want to set it up myself

The React website has a list of [Starter Kit](https://reactjs.org/community/starter-kits.html) projects that can help you get up and running quickly!

*Update 11/10/2018: Originally I was referring to the [Fountain](https://github.com/FountainJS/generator-fountain-react) project, but this doesnÔÇÖt seem to be recently maintained and the original website domain no longer belongs to them. Use with caution.*

### Avoiding JS classes

The above examples should work fine in any modern browser, but if youÔÇÖre conscious of avoiding all potentially-unsupported ES6 code including classes, you can just replace the definition of ÔÇ£GreetingsÔÇØ with the following:

<iframe src="https://medium.com/media/8525d6e2a31e2314ce815ea5c275ef26" frameborder=0></iframe>

React seems to be discouraging the use of React.createClass for the future, but for now you can use it to define React components with object definitions instead of straight-up classes.

For more details on avoiding ES6 code in your samples, consult [React Without ES6](https://facebook.github.io/react/docs/react-without-es6.html)*.*

*Sources: [React Without ES6](https://facebook.github.io/react/docs/react-without-es6.html), [React Without JSX](https://facebook.github.io/react/docs/react-without-jsx.html), [React Installation (Add React to an Existing App)](https://reactjs.org/docs/add-react-to-an-existing-app.html)*
