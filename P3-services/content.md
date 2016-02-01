---
title: "Angular Services"
slug: angular-services
---

<hr><br>
## Learning Objectives
  * Strategize on Comparing Languages and Frameworks
  * Compare and Contrast Front End Frameworks
  * Understand $scope and module injection

<hr>

### Chapter Summary

Angular services are for DRYing up and refactoring code. Angular services are technically "singletons", meaning that they are snippets of code that you would otherwise repeat here and there in the controllers of your app.

Services are great for isolating the code you use to interact with your server and we'll see how we can do that better in the `$resource` lesson. Some other uses of services could be to group together all your functions dealing with authentication and authorization, or maybe housing the functions that call to a specific external API like Google Maps.


# Factories, Services, Providers, Oh My!

When you are reading about angular services you will see the words **factory** and **provider** used synonymously with **service**. Don't worry about it. Technically all services and factories are sorts of producers. Services and Factories are functionally equivalent and vary only in their syntax. Producers are usually unnecessary and you won't see them.

In a true **service** you will see the `this` keyword.

```js
app.service('MyService', function () {
  this.sayHello = function () {
    console.log('hello');
  };
});
```

In a **factory** flavored service look for `return`.

```js
app.factory('MyService', function () {
  return {
    sayHello: function () {
      console.log('hello');
    };
  }
});
```

# Services for Mocking Data

Let's use a pattern that the Ionic Framework uses to simplify mocking data. Here's an example of `Chats` data mocked in an angular service.

```js
angular.module('myApp.services', [])

.factory('Chats', function() {
  // Might use a resource here that returns a JSON array

  // Some fake testing data
  var chats = [{
    id: 0,
    name: 'Ben Sparrow',
    lastText: 'You on your way?',
    face: 'img/ben.png'
  }, {
    id: 1,
    name: 'Max Lynx',
    lastText: 'Hey, it\'s me',
    face: 'img/max.png'
  }, {
    id: 2,
    name: 'Adam Bradleyson',
    lastText: 'I should buy a boat',
    face: 'img/adam.jpg'
  }, {
    id: 3,
    name: 'Perry Governor',
    lastText: 'Look at my mukluks!',
    face: 'img/perry.png'
  }, {
    id: 4,
    name: 'Mike Harrington',
    lastText: 'This is wicked good ice cream.',
    face: 'img/mike.png'
  }];

  return {
    all: function() {
      return chats;
    },
    remove: function(chat) {
      chats.splice(chats.indexOf(chat), 1);
    },
    get: function(chatId) {
      for (var i = 0; i < chats.length; i++) {
        if (chats[i].id === parseInt(chatId)) {
          return chats[i];
        }
      }
      return null;
    }
  };
});

```

We add this service to our app by injecting it into `angular.module()` in `app.js`

```js
angular.module('myApp', ['myApp.services'])
```

# Challenge
1. In your reddit-angular app move your `posts` data into a `Posts` service.
2. Inject your `Posts` service into your controller and call `Posts.all()` to get all the posts.

> If you are prototyping another app, create a service for your core resource or resources and inject them into your controller.

## Putting $resource into Services

In a future lesson we will replace this mocked data with an external angular service called `$resource` which transforms services into easily-injectable connections to your API.
