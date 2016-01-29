# Angular 2

So . . . Angular 1 is basically out of date now. Time to learn Angular 2.

## Compare Contrast - Concepts

1. Forget Controllers - Use Components
  Components are shaped like a tree. e.g. here would be the component structure of of a tabbed hybrid mobile app:
    ```
      - @PictureApp
        - @Tabs
          - @Pictures
            - @Picture
          - @Camera
            - @AddFilter
              - @Share
          - @Profile
            - @Settings
    ```
1. ES6 ([List of Features](https://github.com/lukehoban/es6features)) - Some Highlights:
  - Declaring Variables: Use `let` (not `var`)
  - Classes - That's right just like OO languages
    ```js
    class MillerLite extends Beer {
      constructor() {
        super();
        this.deliciousness = 10;
        this.locations = 'anywhere';
        this.name = 'Miller Lite';
      }
      sell(location) {
        if(Location.hasSportsTeam(location)) {
          return true;
        }
        // Enh, we'll sell anywhere
        return true;
      }
    }
    ```
  - Arrow Functions (for declaring anonymous functions)
    ```js
    class MyClass {
      constructor() {
        this.name = 'Max';

        setTimeout(() => {
          // This prints "Max" since arrow functions bind to our current "this" context.
          console.log(this.name);
        });
      }
    }
    ```
  - Template Strings
    ```js
    let template = `
      <div>
        <h2>Rufferford's Travels</h2>
        <p>
          A most gripping tale of one dog's quest
          for more flavors.
        </p>
      </div>
    `;
    ```
  - String Interpolation
    ```js
    let x = 5;
    let y = 10;
    let template = `
      <div>The sum is <span>${ x + y }</span></div>
    `;
    ```


1. TypeScript
  - https://docs.google.com/document/d/11YUzC-1d0V1-Q3V0fQ7KSit97HnZoKVygDxpWzEYW0U/edit
1. Shadow DOM
1. Components

  Components

  One way to think about component architecture is to contrast folder/file structure in an Angular 1 and Angular 2 app. A traditional file structure for Angular 1 looked like this:

  ```
  |-www/
  |
  |--js/
  |--|-app.js
  |--|-HomeCtrl.js
  |--|-DetailCtrl.js
  |
  |--templates/
  |--|-Home.html
  |--|-Detail.html
  |
  |-index.html

  ```
  But in Angular 2 which is based on **components** you will want to group together code by component.
  ```
  |-www/
  |
  |--Home/
  |--|-HomeCtrl.js
  |--|-Home.html
  |
  |--Detail/
  |--|-DetailCtrl.js
  |--|-Detail.html
  |
  |-index.html
  |-app.js
  ```

## Compare Contrast - Syntax

#### ngRepeat => ngFor

Angular1

```html
```

Angular2
```html
<ul list-group>
  <li list-group-item *ng-for="#user of users" (click)="goToUser(user)"></li>
</ul>
```

#### Navigation

```html
<a href="#/users/{{user._id}}">{{user.name}}</a>
```
