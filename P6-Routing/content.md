---
title: "Routing in Angular"
slug: routing-in-angular
---

<hr><br>
## Learning Objectives
  * Familiarize yourself with the background in Angular routing
  * Contact the Giphy API with `$http`

<hr>

### Chapter Summary

In a client-side framework like AngularJS routing is managed not by the server, but by the front end itself. Angular detects the path of your URL and conventionally maps that URL to a controller and template.

# Background

Angular first shipped with a simple routing module that let you connect one URL with one controller and one template. Very quickly developers in the Angular community wanted to go beyond this simple coupling and leverage Angular's ability to nest controllers, views, and scopes together to make more complex and modular front-end applications. The Angular community decided to extract the standard angular routing module into its own separate module called `ngRoute`. This commenced a virtual food fight for who could build the best routing module for Angular. The unproclaimed winner was a module called `ui-router` which is part of a larger library called  [Angular-UI](https://angular-ui.github.io/) that provides a whole family of useful external Angular services and directives.

For the purposes of learning Angular, we will be using the standard `ngRoute` routing module. But as you google routing problems you will find many examples of apps using `ui-router` so we will look at `ui-router`'s basic setup.

> AngularUI Router is a routing framework for AngularJS, which allows you to organize the parts of your interface into a state machine. Unlike the $route service in the Angular NgRoute module, which is organized around URL routes, UI-Router is organized around states, which may optionally have routes, as well as other behavior, attached.
(From [angular-ui router site](http://angular-ui.github.io/ui-router/site/#/api/ui.router))

The core difference between `ng-route` and `ui-router` is that `ng-route` is based on URL and `ui-router` is based on the `state` of an application and treats the URL as merely an attribute of a state.

We'll be using `ui-router` because it can do everything `ng-route` can do, but `ng-route` can't do everything `ui-router` does. It is also good to note that advance usage of `ui-router` allows for easily nesting routes and templates.

# ngRoute vs ui-router

## Vanilla ng-route

> Reference: [ngRoute](https://docs.angularjs.org/api/ngRoute)


> **Notice** that ngRoute tracks only URL and ui-router tracks state and url is just an attribute of state.

```js
sampleApp.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider
      .when('/phones', {
        templateUrl: 'partials/phone-list.html',
        controller: 'PhoneListCtrl'
      })
      .when('/phones/:phoneId', {
        templateUrl: 'partials/phone-detail.html',
        controller: 'PhoneDetailCtrl'
      })
      .otherwise({
        redirectTo: '/phones'
      });
  }]);
```

## Vanilla ui-router

> Reference: [ui-router](https://github.com/angular-ui/ui-router)

```js
$stateProvider
  .state('home', {
    url: '/',
    templateUrl: 'welcome/home.html',
    controller: 'HomeCtrl'
  })
  .state('guitar-details', {
    url: '/guitars/:guitarId',
    templateUrl: 'guitars/guitar-details.html',
    controller: 'GuitarDetailsCtrl'
  })

$urlRouterProvider.otherwise('/');
```

> **Notice** that there is always an `otherwise()` function to define a fallback route/state if the requested URL does not match any defined route/state.

# Add `ui-router` to a Project

1. Use bower to install `ui-router` and include it in your index.html file after `angular.js`.
  
  ```
  $ bower install angular-ui-router
  ```

  `index.html`

  ```html
    <script src="vendor/angular-ui-router/release/angular-ui-router.min.js"></script>
  ```

1. Now you need to inject the `ui.router` module into your application.
  
  ```js
    angular.module('myApp', ['ui.router'])
  ```

1. Now we're ready to set up our states!

# Setting up our Templates

We're going to turn our `index.html` into a **Layout Template**. Like in Rails, this template will be the common template for all the views in our application. Other **Partial Templates** are then included into this layout template depending on the current state.

1. Create a folder in the top level of your app called `templates`.
1. Create a new file called `posts.html` and put it in the `templates` folder.
1. In `index.html`, move the code between the `<body></body>` tags to your new `posts.html` file.
1. In `index.html` add an `ui-view` directive in the `body`.

  ```html
  <body ng-controller="MainCtrl">
        <div ui-view></div>
  </body>
  ```

# Configure our States

1. All states are defined in the `app.js` file. Other code we will eventually move into other files.
1. Using the `.config()` method, we request the `$routeProvider` to be injected into our config function and use the `$routeProvider.when()` method to define our routes. Open up your `app.js` file. We're going to dot-chain our route information off our app. Put this code at the top of the file right below where you define the app.

  ```js
  angular.module('myApp', ['ui.router'])
    .config(['$stateProvider' '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
      $stateProvider
        .state('home', {
          url: '/',
          templateUrl: 'welcome/home.html',
          controller: 'HomeCtrl'
        })
        .state('guitar-details', {
          url: '/guitars/:guitarId',
          templateUrl: 'guitars/guitar-details.html',
          controller: 'GuitarDetailsCtrl'
        })
        ;
      $urlRouterProvider.otherwise('/');
    }]);
  ```

  **Remember** to keep the names of your templates and controllers **RESTful**.

1. Using the above code as a model, add two states to your prototype app.
  - One state that resolves to `/` and serves up your posts controller and the post index template.
  - A second state that resolves to `/post/:postId`, and serves your post controller and the post show template.

# Accessing `:postId` in URL Params

Using `ui-router` we will want to access URL parameters in a controller, like when you want to use an `:postId` to GET a single post. We'll inject `$stateParams` into our controller to access the url parameters.

```js
.controller("GuitarDetailsCtrl", ['$scope','$http','$stateParams', function($scope, $http, $stateParams) {
  var url = "http://localhost:3000/guitars/"

  $http.get(url + $stateParams.guitarID;).then(
    function (response) {
      $scope.guitar = response.data;
    },
    function (response) {
      alert("No guitar found.")
    }
  });

}]);
```

# Setting up Controllers

1. Create a new file in your top level called `controllers.js`.
1. Add the file to your `<head>` tag: `<script src="controllers.js"></script>`
1. Move your `MainCtrl` from `app.js` to this new file.

  ```js
  angular.module('myApp.controllers', [])
    .controller('MainCtrl', ['$scope', '$rootScope', function($scope, $rootScope) {
      ...
    }])
  ```

1. Inject `myApp.controllers` in to your app like this:

  ```js
    angular.module('myApp', ['ui.router', 'myApp.controllers'])
  ```

1. Move or create your `PostsCtrl` in the `controllers.js` file.
1. In your `PostsCtrl`, use `$http` to get your posts and display them on the `posts.html` template when the app resolves to the root path (`/`).
1. Add a link to the title of your posts that links to the `/posts/:postId`.

  > **Heads up!** Angular has a sort of querk that there is a `#` in urls. So when you want to declare a relative path, you have to prefix it with `#`. e.g. `#/posts/:postId`.

1. Move or create your `PostCtrl` in the `controllers.js` file.

  ```js
    .controller('PostCtrl', ['$scope', function($scope) {

    }])
  ```

1. Move your `createPost` logic and form template into a separate route, controller, and template called `post-new.html`, `NewPostCtrl`, and `/posts/new`.
1. After creating a post, redirect to `/` using the `$location` service. Don't forget to inject the service into your controller...

  ```js
    $location.path('/');
  ```

# Your app

Before moving on, make sure to build out the main states for your app and populate them with data with services.

# CONGRATS! You have Implemented Client Side Routing with AngularJS!
