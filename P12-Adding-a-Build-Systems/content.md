
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
  * Build isomorphic javascript applications

# Tools

There are three categories of build automation tools:

1. Package Management
  - Bower
  - npm
1. Task Runners
  - Grunt
  - Gulp
  - Brunch
1. Dependency Management
  - Webpack
  - Browserify
  - RequireJS/AMD
  - ES6 modules (using Babel to transpile ES6 into ES5)

You can use more and fewer of these in conjunction or use them by themselves. Here are four build system recipes:

1. Bower (just plain old Bower, that's what you have now!)
1. Bower + Gulp or Grunt
1. npm + Browserify + Gul
1. npm + Webpack

# Read Webpack's "Motivation" Article

Webpack has a [great article](http://webpack.github.io/docs/motivation.html) on the motivation for its own existence, but it does a great job of explaining from 10,000 feet why front ends benefit from build systems.

# Task Runners: Gulp & Grunt

The first build system tools we'll look at are *Task Runners*. Task runners do

* [Gulp Tutorial](https://www.youtube.com/watch?v=LmdT2zhFmn4)
* [Grunt Tutorial](https://www.youtube.com/watch?v=TMKj0BxzVgw)


# Browserify

[Browserify](http://browserify.org/) attempts to solve the problem of  
http://www.jeromesteunou.net/browserify-why-and-how.html

# Webpack

https://www.youtube.com/watch?v=9kJVYpOqcVU
