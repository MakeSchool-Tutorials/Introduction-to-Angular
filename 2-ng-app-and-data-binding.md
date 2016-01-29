# Starting our Reddit app

Through data binding, Angular binds variables in `$scope` to the DOM and makes the UI of single page applications multiple times less complex.

Please follow along and make the most basic angular app you can:

1. Create a folder called `reddit-angular`. In the two root make two files: `index.html` and `app.js`.
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

![digestcycle](/images/digest.png)

In jQuery this would be like having a `change()` listener on every single element and a function that updates the DOM correspondingly. Like this but for every element on the page:

```js
  $('input').change(function() {
    $('#term').text($(this).val());
  });
```
