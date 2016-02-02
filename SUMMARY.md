P5-P11 add learning objectives and chapters summary, break up steps into challenges

> [info]
> Blah

> [action]

> [solution]

h1 gets a checkbox at the end of the section "#"

Advanced topics
- gulp
- Angular2
- isomorphic js

## What is Next? Isomorphic Javascript?

There are two major complaints about SPAs:

1. They are not friendly to Search Engine Optimization (SEO) since the robots who crawl the internet don't run the JavaScript to get the actual content from the server.
2. That the initial load of SPA apps is slow.

This has lead to a new paradigm for the web called *Isomorphic Javascript*. In this paradigm developers write packages that run on the server and the client and therefore do both server and client-side templating. This speeds up the initial load and appeases the SEO robot overlords.

Probably the most famous and widely used Isomorphic framework is Meteor. Airbnb made a framework based on Backbone.js called Rendr. ReactJS is a growing templating module that can be extended with Browserify and Webpack to be isomorphic.

# Airbnb on Isomorphic Javascript - 20min

Read [Airbnb's Article on Isomorphic Javascript](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/) and answer these questions:

1. What problems are Isometric Javascript web architectures trying to solve?
1. How does Browserify and Webpack work together to run code on the server and the client?



1. intro.md
2. ng-app-and-data-binding.md
3. ng-controller.md
4. services.md
5. $scope.md
6. $http.md
7. routing.md
8. native-directives.md
9. $resource.md
10. custom-directives.md
12. authentication.md
13. protractor.md

DONE Intro to FE frameworks - why we are studying angular?
DONE Bootstrapping an Angular App - ng-app, ng-model, {{}} $scope
DONE Controllers - ng-app="appName", ng-controller="MainCtl"
DONE Mocking Data with Services
DONE ng-click, ng-submit, $scope.voteUp
DONE $http - Calling APIs & Native Services
DONE Routing - ui-router & NgRoute (needs review)
DONE Native Directives - ng-show/hide, ng-pluralize, ng-include
  - ng-pluralize the number of votes on each post
DONE $resource - replacing mocked data with API calls
DONE External & Custom Directives - ng-file-upload, Weather Directive
DONE AngularJS auth - Token vs. Cookie approach. JWT Auth, Interceptors, & Satellizer
Protractor - what it is how to use it. Needs more advanced test examples and challenges...


Learning Objectives
Base Challenges w Goals
Stretch Challenges w Goals
Elaborative Challenges
