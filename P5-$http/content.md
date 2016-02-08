---
title: "$http"
slug: http
---

<hr><br>
## Learning Objectives
  * Introduction to Native Services
  * Contact the Giphy API with `$http`

<hr>

### Chapter Summary

AngularJS has a number of **Native Services** that are just like the services we were making, but they are shipped with Angular. You can tell a native service because it is prefixed by a `$`. Native services are services so they are singletons that group functions related to a particular action or activity. `$scope` itself is a native Angular service that groups together all the functions and variables exposed in the template. `$rootScope` the parent of all nested scopes is also a native service.

The `$http` service (as you might suspect) manages functions making HTTP requests. And I bet you can guess the functions that `$http` has inside it...

* get()
* post()
* put()
* delete()

Surprise surprise! Its the HTTP action verbs. Let's use `$http` native service to make queries to the giphy API.


# Giphy API

Let's try to display a random gif on our template. We can put logic in the `MainCtrl` for now.

We can use this webhook to get our random gif.

```js
var url = "http://api.giphy.com/v1/gifs/random?api_key=dc6zaTOxFJmzC"

$http.get(url).then(
  function (response) {
    // SUCCESS
  },
  function (response) {
    // ERROR
  }
)

```

## Promises

Notice here that `$http` returns a promise that is resolved with a `then()` function. The promise returns two callbacks. The first is the **SUCCESS** callback (HTTP status of 200), the second is the **ERROR** callback (HTTP status of 300, 400, or 500).

# Challenges

1. Use the code above to query a random giphy gif and put its url into `$scope.gif`. (Hint - the Giphy API response is kinda gnarly so use `debugger` or a JSON inspector plugin in your browser to navigate the response to get the gif url you want.)
1. Display the gif on your template using the `ng-src` directive inside an `<img>` tag. Remember to use bootstrap's responsive image class.
1. In your prototype project, use `$http` in to get your posts from you rails app and load them on the page. (Hint - you will need to be running `rails s`!)

# Decoupled AngularJS App

Currently your AngularJS project is **Decoupled** from your Rails app, meaning it is a completely separate project and is not served to the client through the rails asset pipeline. The two are connected just through queries your AngularJS project makes to exposed **API webhooks** or **API endpoints** in your Rails app.

A decoupled angular app can be served from a CDN, or in the case of an ionic app, is served from the iOS App Store or Google Play Store.

In the future we will learn how to couple your AngularJS project to your rails app (or any app) by having it be served by the server through the client assets. For now, let's just play with the app decoupled so we get in the habit of strictly dividing the duties of the client and of the server.
