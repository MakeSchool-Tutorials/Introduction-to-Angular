# Angular's Native Directives

Angular is made up of directives that supercharge your html. Some of them you already know like `ng-app`, `ng-click`, `ng-controller`, `ng-repeat`, and `ng-model`. You can also make your own custom directives and we'll learn how to do that in a future article, but Angular ships with a slew of very useful directives. To understand Angular directives better, lets look at some of the basic "native" Angular directives.

## Hiding and Showing

#### `ng-show`
(Hint: use `ng-init` to set a variable upon initialization)

```html
<div ng-init="current_user = { name: 'bob' }"></div>

<div ng-show="current_user">
  {{current_user.name}}
</div>
```

#### `ng-hide`
```html
<div ng-hide="its_nighttime">
  Good Night!
</div>
```

## ng-pluralize

This directive lets you pluralize nouns based on a dynamic `count` variable or expression.

```html
<div ng-init="personCount = 1"></div>

<ng-pluralize count="personCount"
                 when="{'0': 'Nobody is viewing.',
                     'one': '1 person is viewing.',
                     'other': '{} people are viewing.'}">
</ng-pluralize>
```

```
Nobody is viewing
1 person is viewing
4 people are viewing
```

## ng-class

"The ngClass directive allows you to dynamically set CSS classes on an HTML element by databinding an expression that represents all classes to be added." - [AngularJS Docs](https://docs.angularjs.org/api/ng/directive/ngClass)

The `ng-class` directive works a few ways. It can simply take a string, or it can take an object where the key is a class that is added if its pairing value is true. The value can be an expression in `$scope`.

```html
  <style>
    .strike {
      text-decoration: line-through;
    }
  </style>
  <!-- STRING BASED -->
  <div ng-class="[newClass]">Some Text</div>
  <input type='text' ng-model="newClass" value="strike"/>

  <!-- OBJECT-BASED -->
  <p ng-class="{strike: deleted, bold: important, 'has-error': error}">Map Syntax Example</p>
  <button ng-click="deleted = !deleted">Strike!</button>
```

Try adding the above code sample to any template and play with it. Can you add other buttons or input fields or radio buttons to manipulate the classes?

## ng-include

With the `ng-include` directive Angular allows you to use partial templates like we've see with underscore, handlebars, and rails. Try out this example. Careful to remember to include the single quotes inside the double quotes.

```html
  <ng-include src="'navbar.html'"></ng-include>
```
> **Watch out!** if you don't run a server with the npm module `http-server`, you can't have templates be in separate folder, they have to be inline like in underscore.

You can also use `ng-repeat` and `ng-include` together.

```html
  <ng-include src="'article.html'" ng-repeat="article in articles"></ng-include>
```

## Filters

Angular gives you a handful of standard [filters](https://docs.angularjs.org/api/ng/filter).

#### `currency`
Formats a number as a currency (ie $1,234.56). When no currency symbol is provided, default symbol for current locale is used.

```js
{{amount | currency}}
// $1,234.56
```

#### `date`

```js
{{'1288323623006' | date:"MM/dd/yyyy 'at' h:mma"}}
// 10/28/2010 at 8:40PM
```

#### `filter`

A standard filter lets you dynamically filter `ng-repeat` lists. Let's look at the syntax.

The below code outputs a list of people with their phone numbers that we can filter by a search input. The `ng-init` directive initializes the template with the array of data (this is not normally done! Usually this data would come from a controller). The `searchText` variable in $scope becomes our filter in the `ng-repeat` directive.

```html
<div ng-init="friends = [{name:'John', phone:'555-1276'},
                         {name:'Mary', phone:'800-BIG-MARY'},
                         {name:'Mike', phone:'555-4321'},
                         {name:'Adam', phone:'555-5678'},
                         {name:'Julie', phone:'555-8765'},
                         {name:'Juliette', phone:'555-5678'}]"></div>

<label>Search: <input ng-model="searchText"></label>

<table>
  <tr>
    <th>Name</th>
    <th>Phone</th>
  </tr>
  <tr ng-repeat="friend in friends | filter:searchText">
    <td>{{friend.name}}</td>
    <td>{{friend.phone}}</td>
  </tr>
</table>
```

# Challenges

## Base Challenges

**Goal: Build out your reddit app to gather and display richer data about posts, in a more user-friendly format.**

1. Pluralize the message for your post count. e.g. "4 Posts" or "There are no posts yet"
1. Use your posts' `created_at` attribute and use the `date` filter to display this time in a format like "posted on Monday April 24, 2015". Hint: use `new Date()`.
1. Move your individual post formatting into a partial called `_post.html`, and use the `ng-include` directive to format posts as you iterate over them.

**Goal: Update your post form to properly validate user data and display errors.**

1. Update your post form to use `ng-class` for when an input is `$invalid` it should have the `error` class.
1. Use `ng-show` or `ng-if` to display error message `.alert` if there are returned 400, 300, or 500 messages.

**Goal: Visually distinguish between voted and unvoted on posts.**

1. Use `ng-class` to add a `voted-on` class if the current user has voted on a post that changes the color of the vote button. (Hint: you will need some part of your data structure to reflect to the client if the current user has voted or not. How could you achieve this?)

## Stretch Challenge: Filter Posts by Category

**Goal: Set up categories of posts.**

1. Add an attribute to your posts that gives each post a `category`, and decide 2-3 categories of posts your users might have. e.g "humor", "science", "funny".
1. Make these categories links that run a function that changes the filter.

**Goal: Allow the user to filter the categories of posts displayed.**

1. Use `filter` to display only one category of post on the page.  
1. Add a radio button selector where the user will be able to select which category of posts to view. Include an "All" button that displays all posts.
1. Instead of a radio button, convert to a checklist where the user can select one or more categories of posts to display.

**Goal: Allow the user to create their own categories/categories of posts.**

1. Add a new form that lets the user add new categories. Validate that the user is not adding blank kinds.
2. Validate that the user is not adding a category that already exists.
2. When a user adds a category, make that category available as a selectable radio button on the new post form.
