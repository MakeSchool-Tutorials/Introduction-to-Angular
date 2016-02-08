---
title: "Authenication"
slug: authentication
---

<hr><br>
## Learning Objectives
  * Compare and contrast a cookie-session and webtoken authentication
  * Using [Satellizer](https://github.com/sahat/satellizer) to deploy JWT in AngularJS

<hr>

### Chapter Summary

> **NOTE** Without a backend we will not be able to fully implement authentication!

With Ruby we learned the Cookie-Session method of authentication; however, there is a better way to do communicate authentication with **Single Page Applications** and a **Service-Based Architecture**. We're going to use an encrypted chunk of JSON called a **JSON Web Token** or JWT (pronounced ''*jot*'') to communicate authentication between client and server.

![cookie-token-auth](cookie-token-auth.png)
> Reference [auth0.com](https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/)

# Authentication with JWT (JSON Web Tokens) Readings

1. Read this excellent article by Max at Ionic about [AngularJS Authentication](http://blog.ionic.io/angularjs-authentication/)
1. Read this [comparison of cookie/session auth and token auth](https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/)

# Why Use JWT?

Aren't you tired of worrying about keeping track of all these things?

1. Sessions - JWT doesn't require sessions
1. Cookies - JWT you just save the token to the client
1. CSRF - Send the JWT instead of a CSRF token
1. CORS - Forget about it, if your JWT is valid, the data is on its way

Also these benefits:

1. Mobile Ready - Apps don't let you set cookies but they can save auth tokens
1. Speed - you don't have to look up the session
1. Storage - you don't have to store the session
1. Testing - you don't have to make logging in a special case in your tests, just send the token.

# JWT FTW

A JWT is pretty easy to identify. It is three strings separated by .

```
  aaaaaaaaaa.bbbbbbbbbbb.cccccccccccc
```

Each part has a different significance:

![jwt](jwt.png)

### Here is a JWT Example:

#### Header

```js
var header = {
  "typ": "JWT",
  "alg": "HS256"
}
```

#### Payload

```js
var payload = {
  "iss": "scotch.io",
  "exp": 1300819380,
  "name": "Chris Sevilleja",
  "admin": true
}
```

#### Signature

```js
var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload);

HMACSHA256(encodedString, 'secret');
```

> **REMEMBER** The 'secret' acts as an encryption string known only by the two parties communicating via JWT. Protect your secrets!

#### JSON Web Token
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzY290Y2guaW8iLCJleHAiOjEzMDA4MTkzODAsIm5hbWUiOiJDaHJpcyBTZXZpbGxlamEiLCJhZG1pbiI6dHJ1ZX0.03f329983b86f7d9a9f5fef85305880101d5e302afafa20154d094b229f75773
```

## Further Reading

- [jwt.io](http://jwt.io/)
- [Atlassian JWT docs](https://developer.atlassian.com/static/connect/docs/latest/concepts/understanding-jwt.html)
- [scotch.io tutorial](https://scotch.io/tutorials/the-anatomy-of-a-json-web-token)
- [JWT Express Node Mongoose](http://blog.matoski.com/articles/jwt-express-node-mongoose/)

# Angular Interceptors

An Angular interceptors allow you to "intercept" http requests and responses and change them. We use an interceptor to attach the JWT to every outgoing http request, and handle what to do with 401 (Unauthorized) statuses in any http response.

```js
// SERVICES.JS

app.factory('authInterceptor', function ($rootScope, $q, $window) {
  return {
    request: function (config) {
      config.headers = config.headers || {};
      if ($window.localStorage.jwtToken) {
        config.headers.Authorization = 'Bearer ' + $window.localStorage.jwtToken;
      }
      return config;
    },
    response: function (response) {
      if (response.status === 401) {
        // handle the case where the user is not authenticated
      }
      return response || $q.when(response);
    }
  };
})

app.config(function ($httpProvider) {
  $httpProvider.interceptors.push('authInterceptor');
});
```

# JWT Flow
> Reference: [blog.matoski.com](http://blog.matoski.com/articles/jwt-express-node-mongoose/)

**Login**

1. Client logs in with email and password.
1. Server checks email and password and if valid sends back a JWT.
1. Client receives the JWT and stores it in localStorage.
1. Client makes requests with the token (this happens automatically using an **AngularJS Interceptor**)
1. JWT is decoded on the server, and the server uses the token data to decide if user has access to the resource.

**Signup**

1. Client sends in email and password.
1. Server creates a new user if everything is valid and sends back a JWT.
1. Client receives the JWT and stores it in localStorage.
1. Client makes requests with the token (this happens automatically using an **AngularJS Interceptor**)
1. JWT is decoded on the server, and the server uses the token data to decide if user has access to the resource.

# Satellizer

[Satellizer](https://github.com/sahat/satellizer) hides a lot of the complexity of using JWT tokens. This is both a good and a bad thing. Let's not lose sight of how Satellizer is helping us.

1. Generating the JWT token (resources/auth.js)
1. Making some routes require authentication:

  ```js
  app.get('/api/me', auth.ensureAuthenticated, function(req, res) {
    User.findById(req.userId, function(err, user) {
      res.send(user);
    });
  });
  ```
1. Sending the JWT with every request using an angular interceptor.


# Challenges

**Add Satellizer**

1. Use Bower to install Satellizer
1. Add the module to your app.

  ```js
    angular.module('MyApp', ['satellizer'])
  ```

1. Now inject `$auth` into whatever controller you want to do Authentication stuff. I recommend handling authentication stuff in your `MainCtrl`. Review the [`$auth` API Reference](https://github.com/sahat/satellizer#api-reference)

1. Review the [Email and Password Authentication](https://github.com/sahat/satellizer#-login-with-email-and-password) pattern in the satellizer docs.

1. Now we've looked at the client side of JWT authentication with Satellizer. For the server side, look at the satellizer [Ruby Server Example](https://github.com/sahat/satellizer/tree/master/examples/server/ruby) to see how to serve JWT from your Rails Server. The files you will have to look at are:
  * Gemfile
  * application_controller.rb
  * auth_controller.rb
  * models/token.rb

# Stretch Challenges

1. Add Facebook authentication with Satellizer.
