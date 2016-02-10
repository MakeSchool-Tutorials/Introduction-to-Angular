---
title: "ngApp and Data Binding"
slug: ngapp-and-data-binding
---

<hr><br>
## Learning Objectives
  * Become a 100x Engineer
  * Project Planning Strategies
  * Bootstrapping an Angular App with CDN
  * Adding Bower

<hr>

### Chapter Summary

Remember that one of the benefits of front end frameworks is the strict separation of concerns between front end and back end. We're going to leverage this benefit to build a *decoupled* AngularJS prototype app with mocked data, sometimes called *vaporware*. Once we have a prototype working well, we can attach it to our rails API.

But before we start coding the prototype we have to ask ourselves one question:

# How to Be a 100x Engineer? - 30min

In the industry people are generally thought of 1x, 10x, and 100x engineers. Read [this article](https://hbr.org/2011/06/why-zuckerberg-is-almost-right.html).

Pair up with one or two other people and discuss:

> What makes a developer be 100x more productive than a normal engineer? What do you do that accelerates you?

## Planning

Because we want to be 100x developers, there is one skill set that is an easy way to accelerate our development speed and that is good planning **before we star to code**. Of course there is inside all of us the desire to clench our fists around our keyboard and mouse, close our eyes, and in a sustained fit of nerd-rage pinwheel our hands at our computer screen until our app works; however, instead of thinking of planning as a boring and useless waste of time, think of planning this way:

>It is 10x easier to make changes in a user narrative than on a wireframe; 10x easier to make changes in a wireframe than in development; 10x easier to make changes in development than in production. So it is 1000x easier to make changes in user narratives than in production.

We don't want to plan every detail, and we don't want to spend a lot of time planning (the whole point is to speed up!). So the most basic planning we can do are the following:

1. Come Up With an Idea
1. Write User Narratives
1. Draw Wireframes for your Main Pages and Navigation
1. Draw a SQL Schema Drawing

# Come Up with an Idea - 30min

For the sake of this project think of a B2B or B2C web product. If you are feeling up for a challenge, use the [ionic framework](http://ionicframework.com/) to build a hybrid mobile app.

If you don't have something you've always wanted to build and can't think of anything new, you can always build a prototype of a  Reddit clone.

# Write User Narratives - 20min

User narratives are stories where you **get inside the heads of your users**. They are **NOT A LIST OF FEATURES**.

Here's an example for a user narrative for a "Yelp for Landlords"


> [info]
> Hi, I'm Randy the Renter and I'm looking for housing in San Francisco. I found 5 or 6 places on craiglist to live, but I want to see if the landlord is good because I've had trouble in the past with bad landlords. I go to "yelpforlandlords.com" and search by the address of the unit. The landlord comes up. It looks like they are a good landlord, 5 stars and based on the underlying property value, she doesn't gauge the renters. Awesome!


By fully **getting inside the head of a user** you can build an app that people actually want to use. This techinque can be used for hardware, software, even when you are building esoteric software development libraries, you always have a user that you want to make happy.

Another example:


> [info]
> Hi, I'm Larry the Landlord and I'm a great landlord and want to read my reviews on yelpforlandlords.com. I visit the page and I look up my name. I find that I have many good reviews and I leave responses like "Thanks! You were a great tenant". On one negative review I try to give context to the problem they had.


Write your user narratives for your prototype.

# Wireframe Your Main Pages and Navigation - 20min

There is all kinds of fancy software to make wireframes of the UI/UX of your prototype, but nothing beats a good old pen and paper. That way you can move fast.

Draw out in very little detail the major pages of your site. Focus on flow and navigation.

# Draw a SQL Schema Drawing - 10min

Think about your data structure. What are your major resources? What are their associations with other resources? What are the attributes of your resources?

![schema](http://i.stack.imgur.com/hbUy5.png)

Sketch up your schema.

# Bootstrap Angular - 20min

Through data binding, Angular binds variables in `$scope` to the DOM and makes the UI of single page applications multiple times less complex.

Please follow along and make the most basic angular app you can:

1. Clone the empty [github classroom repo](https://classroom.github.com/assignment-invitations/28340fa8482ea139999b48400955ebab).
1. Head over to [gitignore.io](https://www.gitignore.io) to generate a OSX .gitignore file. (Watch [this video](https://vimeo.com/70609819) to learn more about .gitignore files.)
1. `$ touch .gitignore` to create a `.gitignore` file and paste in your OSX Gitignore you generated.
1. Create a folder for your prototype. In the two root make two files: `index.html` and `app.js`.
2. For now, ignore `app.js`. In `index.html` add the below code.
2. In your terminal begin a simple http server. (You can use one you are familiar with or use this command `sudo npm install http-server -g` and call `http-server`). Now open `http://localhost:8080` in your browser.
3. Type in the input. What do you see?

  ```html
  <!doctype html>
  <html ng-app>
  <head>
    <title>My Angular App</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.4/angular.min.js"></script>
  </head>
  <body>

    <input type="text" ng-model="term" />
    <p>{{term}}</p>

  </body>
  </html>
  ```

  What's going on here?
  * `ng-app` is initializing angular on the `<html>` tag.
  * The `<script . . .` tag is grabbing Angular 1.4.4 from a content delivery network.
  * The `ng-model` native angular directive is binding the value of the input field to the scope variable `term`
  * We are displaying the value of `term` with handlebar brackets `{{}}`

This is what is called **two way data binding**. As you type, you update `$scope` Angular's data model. Every time the model is updated, Angular updates the views to match the new state of the model. This is called Angular's **Digest Loop**.

![digestcycle](../images/digest.png)

In jQuery this would be like having a `change()` listener on every single element and a function that updates the DOM correspondingly. Like this but for every element on the page:

```js
  $('input').change(function() {
    $('#term').text($(this).val());
  });
```

# Bower

We could just use that Content Delivery Network (CDN) link going forward, but in production you'll want to use a frontend package manager like [Bower](http://bower.io/). Bower is a tool twitter made to make it as easy to manage front end packages as Bundler and npm make it to manage back end packages.

1. Install bower globally on your machine and initialize bower (just hit enter for the default options).

  ```
  $ npm install bower -g
  $ bower init
  ```

1. Let's add a hidden [bower configuration](http://bower.io/docs/config/) file so we can change the directory where our bower packages save:

  ```
  $ touch .bowerrc
  ```

  `.bowerrc`
  
  ```
  { "directory" : "/vendor" }
  ```

1. Now let's install our first package: Angular

  ```
  $ bower install angular -save
  ```

1. You should see a `vendor` folder with the angular assets inside. Whoohoo! Next change the CDN link to hit the angular.min.js file in your `vendor` folder.

  ```html
  <head>
    <title>My Angular App</title>
    <script src="vendor/angular/angular.min.js"></script>
  </head>
  ```

1. remember to add `vendor` to your `.gitignore` - heroku will run `bower install` upon deployment and get fresh versions of your bower packages in production.

## Nice Work! On to The Next Article
