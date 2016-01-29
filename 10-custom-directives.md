# Custom & External Angular Directives

So we've used and covered `.config()`, `.controller()`, and `factory()`, now we will look at `.directive()`.

Directives are another way to put related code together and make a self-contained component or a widget. Directives are snippets of html with their own custom JavaScript logic. Directives are not necessary or required for any app, but they are a useful and unique part of AngularJS 1.x, and if your app design includes a few components, it might be nice to use directives to separate concerns and DRY up your controllers.

An analogy for directives is if you think of the views of your app like a butterfly collection. Each butterfly is a directive. Angular's concept of directives helps separate concerns and duties of code while making your views dry and logic-less. Also Angular directive are very modular and can be added, shared, and swapped between projects.

![butterflys](/images/butterflies.jpeg)

Checkout [angular-ui](https://angular-ui.github.io/) and [ng-modules](http://ngmodules.org/) to find popular Angular Directives and add them to your projects.

## Adding an External Directive

Sometimes when you are looking to solve a problem you find a solution has already been made in the form of a directive. Now the challenge is how to include that directive in your project.

1. First, add the files to your project
2. Include them in `index.html`
3. Inject the directive into your app.
  ```js
    angular.module('yourApp', ['ngMap', 'pickadate', 'ui.bootstrap'])
  ```
4. Now they will be ready to use in your templates.

Simple as that!

## Making Your Own Directive

### A Current Weather Example

**Follow Along** by putting these samples into an Angular project.

> Reference: [ng-newsletter blog post on directives](http://www.ng-newsletter.com/posts/directives.html)

Imagine you wanted to make a box that displayed the current weather of a city and reuse the same snippet on various pages for various cities. A directive would be a great solution! Let's look at how you'd build this directive that fetches weather data about a city and displays the weather.

Place this HTML anywhere in your templates:
```html
<current-weather city="San Francisco"></current-weather>
```

Add this directive to your app:
```js
app.directive('currentWeather', function() {
  return {
    restrict: 'E',
    scope: {
      city: '@'
    },
    template: '<div class="current-weather"><h4>Weather for {{city}}</h4>{{weather.main.temp}}</div>',
    //templateUrl: 'templates/current-weather-template.html',
    //transclude: true,
    controller: ['$scope', '$http',
      function ($scope, $http) {
        var url = "http://api.openweathermap.org/data/2.5/weather?mode=json&cnt=7&units=imperial&callback=JSON_CALLBACK&q="
        $scope.getWeather = function(city) {
          $http({ method: 'JSONP', url: url + city })
            .success(function(data) {
              $scope.weather = data;
            });
        }
    }],
    link: function (scope, element, attrs) {
      scope.weather = scope.getWeather(attrs.city);
    }
  }
});
```

## Angular Directive Options

#### Restrict

The first option in an Angular directive is the `restrict` option. This option let's you specify how exactly you'd like to call the directive in HTML. Here are your options: A and E are the most popular.

```html
'A' - <span ng-sparkline></span>
'E' - <ng-sparkline></ng-sparkline>
'C' - <span class="ng-sparkline"></span>
'M' - <!-- directive: ng-sparkline -->
```

#### template and templateUrl

Using the `template` and `templateUrl` options you can define an HTML template inside the directive's JS or in a separate HTML file in the templates folder.

#### Scope inside a Directive

But wait a sec, how do directives interact with the `$scope` set by the local controller? Can I get data from the local controller into my directive?

By default scopes do inherit the scope of their local controller just like they were HTML in the template. However you can use the `scope` option to change this default behavior to isolate your directive's scope.

1. `scope: true` - if scope is set to `true` then the directive will have its own child scope that inherits from the parent scope of the local controller, meaning it can still access and change the parent scope.
2. `scope: {}` - by passing an object to the `scope` option, you can define an **isolated scope**. Inside this object you can pass in three **aliases**.
  * `@` - sets a local string
  * `=` - creates a bi-directional binding, very similar to if the directive's scope participated directly in the local controller's scope, but only regarding one variable.
  * '&' - this alias gives you access by reference to a method in the local controller.

Here is an example

```html
<input type="text" ng-model="to" />
<!-- Invoke the directive -->
<div scope-example ng-model="to" on-send="sendMail(email)" from-name="ari@fullstack.io" />
```

In the directive you would access the values of these three attributes like this:
```js
scope: {
  ngModel: '=',     // Bind the ngModel to the object given
  onSend: '&',      // Pass a reference to the method
  fromName: '@'     // Store the string associated by fromName
}
```

#### controller

The controller option allows you to define a controller specific and isolated to the directive.

#### link()

The `link()` option is the meat and potatoes of the directive. Inside this function you determine what you would like to be done and you can update scope.

#### transclude -- (WTF!?)

Don't be afraid of this fancy sounding word. It means that you can pass in a variable inside the directive parent element. Let's look at an example from the [Angular ngTransclude docs](https://docs.angularjs.org/api/ng/directive/ngTransclude).

The `{{text}}` inside the `<pane></pane>` directive parent html element will be rendered inside the `<ng-transclude></ng-transclude>` directive inside the `pane` directive template.

```html
<script>
  angular.module('transcludeExample', [])
   .directive('pane', function(){
      return {
        restrict: 'E',
        transclude: true,
        scope: { title:'@' },
        template: '<div style="border: 1px solid black;">' +
                    '<div style="background-color: gray">{{title}}</div>' +
                    '<ng-transclude></ng-transclude>' +
                  '</div>'
      };
  })
  .controller('ExampleController', ['$scope',
    function($scope) {
      $scope.title = 'Lorem Ipsum';
      $scope.text = 'Neque porro quisquam est qui dolorem ipsum quia dolor...';
    }
  ]);
</script>
<div ng-controller="ExampleController">
  <input ng-model="title" aria-label="title"> <br/>
  <textarea ng-model="text" aria-label="text"></textarea> <br/>
  <pane title="{{title}}">{{text}}</pane>
</div>
```

# Challenges

## Base Challenges

1. Make a new Angular app, and chain the `currentWeather` directive to your app in the `app.js` file.
2. Pull out the template into a different file and use the `templateUrl` option.
3. Improve the template and show more information from inside the `weather` JSON.
4. Add the [angular-validation-match](https://github.com/TheSharpieOne/angular-validation-match) directive to validate that a password field matches a password confirmation field. **Hint:** Add the [source file](https://github.com/TheSharpieOne/angular-validation-match/blob/master/dist/angular-validation-match.min.js) to your app, then link to it using a `<script>` tag in your `index.html`. You'll also need to add `'validation.match'` as a dependency in your Angular app.

5. Add the [ngMap](https://github.com/allenhwkim/angularjs-google-maps) directive to display a map in your app centered on SF. **Hint:** [This blog post](http://allenhwkim.tumblr.com/post/70986888283/google-map-as-the-simplest-way) has some very helpful examples.

## Evening Challenges

1. Create a directive called `five-day-forecast` that fetches and displays the five day forecast from the [openweathermap api](http://openweathermap.org/forecast5).
2. Add the [angular-moment](https://github.com/urish/angular-moment) directive to show a date and time in your app that ticks down by the second.
3. Find and implement an edit-in-place directive.
4. Come up with an idea for a custom directive and implement it. For a bonus, use an external API like the weather example above.
