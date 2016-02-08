---
title: "$scope"
slug: scope
---

<hr><br>
## Learning Objectives
  * Detect click and submit events with ngClick and ngSubmit
  * Implement Angular's Native Form Validation

<hr>

### Chapter Summary

Besides keeping variables in `$scope` we can also expose functions to the template through `$scope`. We can then use native angular directives to have these functions run dynamically on clicks and form submits with `ng-click` and `ng-submit`.

# ngSubmit

## $scope.createPost()

For your Reddit app, you'll need to create posts. For now, posts will just have two attributes called `title` and `voteCount` they will be added from the top of your app. We can always move this into a `post-new.html` template later once we know how to do routing.

Add the form

```html
<form name="postForm" ng-submit="createPost()">
  <input type="text" placeholder="Title" ng-model="post.title">
  <button type="submit">Save</button>
</form>
```

Add the `createPost()` function to `$scope` in the `MainCtl` controller.

```js
  $scope.post = { title: "", voteCount: 0 };

  $scope.createPost = function() {
    $scope.posts.push($scope.post);
    $scope.post = { title: "", voteCount: 0 };
  };
```

What is going on here? What is happening line by line? Write pseudocode comments line by line explaining what each line does.

1. Add a form to your prototype and use `ng-submit` to have the form update `$scope`.


# Native Angular Validations

Angular gives you native validators. Let's use angular form validators and `ng-disabled` to disable the save button unless our form is valid.

Here's an example for a `userForm`. Using this example as a model, add angular form validations to your `postForm` and make your button disabled.

```html
<form name="userForm" ng-submit="submitForm(userForm.$valid)" novalidate> <!-- novalidate prevents HTML5 validation since we will be validating ourselves -->

    <!-- NAME -->
    <div class="form-group">
        <label>Name</label>
        <input type="text" name="name" class="form-control" ng-model="name" required>
    </div>

    <!-- USERNAME -->
    <div class="form-group">
        <label>Username</label>
        <input type="text" name="username" class="form-control" ng-model="user.username" ng-minlength="3" ng-maxlength="8" required>
    </div>

    <!-- EMAIL -->
    <div class="form-group">
        <label>Email</label>
        <input type="email" name="email" class="form-control" ng-model="email" required>
    </div>

    <!-- SUBMIT BUTTON -->
    <button type="submit" class="btn btn-primary" ng-disabled="userForm.$invalid">Submit</button>

</form>

```

By layering on a few conditional css classes and an alert using [ng-if](https://docs.angularjs.org/api/ng/directive/ngIf) and  [ng-class](https://docs.angularjs.org/api/ng/directive/ngClass) you can implement a very delightful, powerful, and flexible form validation. More on these native directives to come.

1. Add angular native validations on submit.

# ngClick

## $scope.voteUp

Using the logic from the `createPost()` section, try to make a `voteUp()` function.

**Hints**

* Add the `ng-click` directive on the element you call `ng-repeat` on.
* Pass in your current `post` as an argument into your function `voteUp(post)`
* Make sure `voteUp` is a function in `$scope`
* What do you do with the `post` once someone votes up? Increment the `voteCount`

> [solution]
>
> ```html
> <div ng-repeat="post in posts" ng-click="voteUp(post)">
>   {{post.title}}
>   <div class='text-right text-muted'>
>     {{post.voteCount}} votes
>   </div>
> </div>
> ```
> 
> ```js
> $scope.voteUp = function(post) {
>   ++post.voteCount
> }
> ```
