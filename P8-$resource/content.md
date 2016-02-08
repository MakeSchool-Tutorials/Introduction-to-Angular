---
title: "$resource"
slug: resource
---

<hr><br>
## Learning Objectives
  * Implement `$resource` in a service

<hr>

### Chapter Summary

Angular’s `$resource` is a native AngularJS service that lets you interact with RESTful backends easily. `$resource` makes your Angular services behave sort of like models in a traditional MVC pattern. In this tutorial, we're going to assume you have a RESTful API for your prototype app and refactor our `$http` requests into services with `$resource`.

> **Reminder**: remember your APIs should be RESTful and respond with proper error messages so they can be consumed easily by modules like `$resource`.

# Benefits of `$resource`

#### 1. Separation of Concerns & Brevity

`$resource` allows you to abstract calls to your API from the controller to services. So this controller code:

```js
var url = "http://localhost:3000/posts"

$http.get(url).then(
  function (response) {
    $scope.posts = response.data;
  },
  function (response) {
    $scope.error = response.data;
  }
)
```

Becomes this one-liner:

```js
  $scope.posts = Post.query();
```

And in your `services.js` file you have only this:

```js
  .factory('Post', ['$resource', '$window', function ($resource, $window) {
    return $resource($window.location.origin + '/api/v1/posts/:id', { id: '@id' })
  }])
```

Notice that the `$window.location.origin` will detect development and production environment root-level domains, so your code will run regardless of development or production instances.

> **Careful!** This will not work with Ionic apps!

# Challenge

## Installation

1. The `$resource` service doesn’t come bundled with the main Angular script. For now add it to your `index.html` file with the CDN. In a production app you'll likely want to use Bower for this.

  ```bash
  $ bower install angular-resource
  ```

  ```html
  <script src="vendor/angular-resource/angular-resource.min.js"></script>
  ```
  
1. Now you need to load the `$resource` module into your application.
  ```js
  angular.module('app', ['ngResource']);
  ```

## Interacting with the API

1. To use `$resource` inside your controller/service you need to inject it into your service, then call the `$resource()` function with your REST endpoint, as shown in the following example. This function call returns a `$resource` class representation which can be used to interact with the REST backend. Create a `services.js` file and put your new `$resource` service in it.

  ```js
  angular.module('myApp').service('Post', function ($resource) {
    return $resource('http://localhost:3000/posts/:id');
  });
  ```

1. The result of the function call is a resource class object which has the following five methods by default: `get()`, `query()`, `save()`, `remove()`, `delete()` (delete is an alias for remove)

1. Let’s see how we can use the `get()`, `query()`, `save()`, and `delete()` methods in a controller using a sample `Book` service:
  ```js
  angular.module('myApp').controller('BooksCtrl', ['$scope', 'Book', function($scope, Book) {
      $scope.book = Book.get({ id: 200 }, function(data) {
        console.log(data);
      }); // get() returns a single book

      $scope.allBooks = Book.query(function(data) {
        console.log(data);
      }); //query() returns all the books

      // add a new book
      $scope.newBook = {"title":"JavaScript: The Good Parts","author":"Douglas Crockford","image":"","release_date":"May 11, 2008"};

      Book.save($scope.newBook, function(data) {
        console.log(data);
      });

      // delete a book
      Book.delete({id:200});
  }]);
  ```

  The `get()` function in the above snippet issues a GET request to `/books/:id`.

  The function `query()` issues a GET request to /api/entries (notice there is no `:id`).

  The `save()` function issues a POST request to `/api/entries` with the first argument as the post body. The second argument is a callback which is called when the data is saved.

1. Use the previous step's sample code to get your posts from your API.

1. Use the previous step's sample code to refactor your new-post controller.

1. Use the `Post.delete()` and `Post.get()` to refactor your delete and show pages.

1. We have explored the create, read and delete parts of CRUD. The only thing left is update. To support an update operation we need to modify our custom service `Book` as shown below.
  ```js
  angular.module('myApp').service('Book', function($resource) {
    return $resource('http://daretodiscover.herokuapp.com/books/:id', { id: '@_id' }, {
      update: {
        method: 'PUT' // this method issues a PUT request
      }
    });
  });
  ```

1. Now we can use the `update` function like this:
  ```js
  var book = Book.get({ id: 200 }, function() {
      book.title = "Updated Title";
      Book.update({id: 200}, book)
  });
  ```

1. Add an edit/update page/flow to your app.
