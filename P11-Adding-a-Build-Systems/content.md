---
title: "Adding a Build System"
slug: adding-a-build-system
---

<hr><br>
## Learning Objectives
  * Review build automation tools
  * Implement a gulp file to concatenate and minify your JS files

<hr>

### Chapter Summary

The front end has a lot of in's and out's, a lot of files and dependencies, but unlike the server the files don't all just sit there waiting to be used. All front end files have to end up running on the user's browser.

There are a lot of fun things like **CoffeeScript** and **Jade** templates that make writing front end code leaner and more elegant, but these eventually need to end up as plain-jane JS, CSS, and HTML so browsers can run them.

*Build Systems* are a set of tools that work together to simplify and automate all this front end complexity.

# Build Systems Benefits

You don't need to have a build system, but there are big advantages to having even a simple one:

  * Improve product quality
  * Accelerate the compile and link processing
  * Eliminate redundant tasks
  * Minimize "bad builds"
  * Eliminate dependencies on key personnel
  * Have history of builds and releases in order to investigate issues
  * Save time and money - because of the reasons listed above
  * Setup continuous integration and continuous testing
  * Build isomorphic or "Universal" javascript application

# Tools

There are three categories of build automation tools:

1. Package Management
  - Bower
  - npm
1. Task Runners
  - Grunt
  - Gulp
  - Brunch
1. Dependency Management / Module Loaders / Bundlers
  - Webpack
  - Browserify
  - RequireJS/AMD
  - ES6 modules (using Babel to transpile ES6 into ES5)

You can use more and fewer of these in conjunction or use them by themselves. Here are four build system recipes:

1. Bower (just plain old Bower, that's what you have now!)
1. Bower + Gulp or Grunt
1. npm + Browserify + Gulp
1. npm + Webpack (What no task runner? npm can be a task runner too!)

# Great Stack Overflow Answer

To make sense of all of this build system craziness - please first read [this Stack Overflow Answer](http://stackoverflow.com/questions/33561272/task-runners-gulp-grunt-etc-and-bundlers-webpack-browserify-why-use-toge)

# Read Webpack's "Motivation" Article

Webpack has a [great article](http://webpack.github.io/docs/motivation.html) on the motivation for its own existence, but it does a great job of explaining from 10,000 feet why front ends benefit from build systems.

# Task Runners: Gulp & Grunt

The first build system tools we'll look at are *Task Runners*. Task runners do you're grunt work, that is probably why the first task runner to explode in popularity was called **Grunt**. More recently **Gulp** has come on the scene and offers a more streamlined way to define, organize, and run your build tasks.

* [Gulp Tutorial](https://www.youtube.com/watch?v=LmdT2zhFmn4)
* [Grunt Tutorial (for reference)](https://www.youtube.com/watch?v=TMKj0BxzVgw)

# Module Loaders (or Bundlers)

# Browserify

As stated in the [Webpack motivation article](http://webpack.github.io/docs/motivation.html), there needs to be a solution to the problem of sending the relevant front end code to the client. [Browserify](http://browserify.org/) attempts to solve the problem of sending the minimum amount of javascript necessary.

The way it works is you can simply use `require()` to add dependencies to your client-side files. **Wait, WHOA?** - that's right, use `require()`.

1. Use `require('angular')` to require dependencies in your client-side javascript files (like you would on the server-side)
1. Run `browserify main.js -o bundle.js`
1. Add `<script src="bundle.js"></script>` to your `<head>`

Read [this article](http://www.jeromesteunou.net/browserify-why-and-how.html) for more information on Browserify

# Webpack

Webpack is like browserify but is for more than javascript. Webpack can replace your task runner and preprocessors. It allows you to preprocess all your various front end modules into vanilla HTML, CSS, & JS. It is the module loader of choice for Reactjs.

1. Watch this [Webpack Tutorial](https://www.youtube.com/watch?v=9kJVYpOqcVU)

# Challenge - Add Gulp to Your Angular App

**Goal - to concatenate and minify your js into one file with gulp**

1. Follow these [Getting Started docs](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)
1. Use [`gulp-concat`](https://github.com/contra/gulp-concat) to concatenate all the javascript files you want to send to the client.
1. Use [`gulp-minify`](https://www.npmjs.com/package/gulp-minify) to minify this concatenated file into a `.min.js` version.
1. Use [`gulp-watch`](https://www.npmjs.com/package/gulp-watch) to watch for changes in your javascript and rerun your concatenate and minify task.

# Compare Contrast Browserfiy and Webpack

Read [this article by Bitnative.com](https://medium.com/@housecor/browserify-vs-webpack-b3d7ca08a0a9#.rfiu01zif)

# Stretch Challenges

1. Can you write some sassy stylesheets and gulp them into regular css?
1. Can you concatenate and minify your stylesheets?

# Extra Stretch

1. Instead of gulp, add webpack to your project.
