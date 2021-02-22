# passport-keycloaksteve

[Passport](http://passportjs.org/) strategy for authenticating with Keycloak
using the OAuth 2.0 API.

This module lets you authenticate using Keycloak in your Node.js applications.
By plugging into Passport, Keycloak authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

[![npm](https://img.shields.io/npm/v/passport-github.svg)](https://www.npmjs.com/package/passport-github)
[![build](https://img.shields.io/travis/jaredhanson/passport-github.svg)](https://travis-ci.org/jaredhanson/passport-github)
[![coverage](https://img.shields.io/coveralls/jaredhanson/passport-github.svg)](https://coveralls.io/github/jaredhanson/passport-github)


## Install

```bash
$ npm install passport-keycloaksteve
```

## Usage

#### Create an Application

Before using `passport-keycloaksteve`, you must register a client on your Keycloak server.
Your client will be issued a client ID and client
secret, which need to be provided to the strategy.  You will also need to
configure a callback URL which matches the route in your application.

#### Configure Strategy

The KeycloakSteve authentication strategy authenticates users using a Keycloak account
and OAuth 2.0 tokens.  The client ID and secret obtained when creating an
application are supplied as options when creating the strategy.  The strategy
also requires a `verify` callback, which receives the access token and optional
refresh token, as well as `profile` which contains the authenticated user's
GitHub profile.  The `verify` callback must call `cb` providing a user to
complete authentication.

```js
var KeycloakSteveStrategy = require('passport-keycloaksteve').Strategy;

passport.use(new KeycloakSteveStrategy({
    clientID: KEYCLOAK_CLIENT_ID,
    clientSecret: KEYCLOAK_CLIENT_SECRET,
    callbackURL: "http://127.0.0.1:3000/auth/keycloaksteve/callback"
  },
  function(accessToken, refreshToken, profile, cb) {
    User.findOrCreate({ githubId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'keycloaksteve'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.get('/auth/keycloak',
  passport.authenticate('keycloaksteve'));

app.get('/auth/keycloaksteve/callback', 
  passport.authenticate('github', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
```

## Examples

Developers using the popular [Express](http://expressjs.com/) web framework can
refer to an [example](https://github.com/passport/express-4.x-facebook-example)
as a starting point for their own web applications.  The example shows how to
authenticate users using Facebook.  However, because both Facebook and GitHub
use OAuth 2.0, the code is similar.  Simply replace references to Facebook with
corresponding references to GitHub.

## Contributing

#### Tests

The test suite is located in the `test/` directory.  All new features are
expected to have corresponding test cases.  Ensure that the complete test suite
passes by executing:

```bash
$ make test
```

#### Coverage

The test suite covers 100% of the code base.  All new feature development is
expected to maintain that level.  Coverage reports can be viewed by executing:

```bash
$ make test-cov
$ make view-cov
```

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2016 Jared Hanson <[http://jaredhanson.net/](http://jaredhanson.net/)>
Copyright (c) 2021 Steve Groom

