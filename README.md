everyauth
==========

Authentication and authorization (password, facebook, & more) for your node.js Connect and Express apps.

`everyauth` is:

- *Modular* - We have you covered with Facebook and Twitter 
  OAuth logins, basic login/password support, and modules 
  coming soon for beta invitation support and more.
- *Easily Configurable* - everyauth was built with powerful
  configuration needs in mind. Configure an authorization strategy 
  in a straightforward, easy-to-read & easy-to-write approach, 
  with as much granularity as you want over the steps and 
  logic of your authorization strategy.
- *Idiomatic* - The syntax for configuring and extending your authorization strategies are
  idiomatic and chainable.
- *Step-driven*

## Installation
    $ npm install everyauth

## Quick Start
Using everyauth comes down to just 2 simple steps if using Connect
or 3 simple steps if using Express:

1. *Choose and Configure Auth Strategies* - Find the authentication strategy
   you desire in one of the sections below. Follow the configuration
   instructions.
2. *Add the Middleware to Connect*
        var everyauth = require('everyauth');
        // Step 1 code goes here
        
        // Step 2 code
        var connect = require('connect');
        var app = connect(
            connect.bodyParser()
          , connect.cookieParser()
          , connect.session({secret: 'mr ripley'})
          , everyauth.middleware()
          , connect.router(routes)
        );
3. *Add View Helpers to Express*
        // Step 1 code
        // ...
        // Step 2 code
        // ...
        
        // Step 3 code
        everyauth.helpExpress(app);
        
        app.listen(3000);

## Setting up Facebook Connect

    var everyauth = require('everyauth')
      , connect = require('connect');
    
    everyauth.facebook
      .myHostname('http://localhost:3000')
      .appId('YOUR APP ID HERE')
      .appSecret('YOUR APP SECRET HERE')
      .findOrCreateUser( function (session, accessToken, fbUserMetadata) {
        // find or create user logic goes here
      })
      .redirectPath('/');
    
    var routes = function (app) {
      // Define your routes here
    };
    
    connect(
        connect.bodyParser()
      , connect.cookieParser()
      , connect.session({secret: 'whodunnit'})
      , everyauth.middleware()
      , connect.router(routes);
    ).listen(3000);

You can also configure more parameters (most are set to defaults) via
the same chainable API:
    
    everyauth.facebook
      .entryPath('/auth/facebook')
      .callbackPath('/auth/facebook/callback')
      .scope('email')                // Defaults to undefined

If you want to see what the current value of a
configured parameter is, you can do so via:

    everyauth.facebook.scope(); // undefined
    everyauth.facebook.entryPath(); // '/auth/facebook'
      

## Setting up Password Authentication
    var everyauth = require('everyauth')
      , connect = require('connect');
    
    everyauth.password
      .getLoginPath('/login') // Page with the login form
      .postLoginPath('/login') // What you POST to
      .loginView('a string of html; OR the name of the jade/etc-view-engine view')
      .redirectPath('/') // Where to redirect to after a login
      .authenticate( function (login, password) {
        // Returns a user if we can authenticate with the login + password.
        // If we cannot, returns null/undefined
      });
    
    var routes = function (app) {
      // Define your routes here
    };
    
    connect(
        connect.bodyParser()
      , connect.cookieParser()
      , connect.session({secret: 'whodunnit'})
      , everyauth.middleware()
      , connect.router(routes);
    ).listen(3000);

## Express Helpers

If you are using express, everyauth comes with some useful dynamic helpers.
To enable them:

    var express = require('express')
      , everyauth = require('everyauth')
      , app = express.createServer();
    
    everyauth.helpExpress(app);

Then, from within your views, you will have access to the following helpers methods
attached to the helper, `everyauth`:

- `everyauth.loggedIn`
- (more - we copy over req.session.auth keys/values to the everyauth helper)

## Configuring a Module
everyauth was built with powerful configuration needs in mind.

### License
MIT License

---
### Author
Brian Noguchi
