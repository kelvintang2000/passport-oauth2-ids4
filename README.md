# passport-oauth2-ids4

[![DepShield Badge](https://depshield.sonatype.org/badges/owner/repository/depshield.svg)](https://depshield.github.io)

General-purpose OAuth 2.0 authentication strategy for [Passport](http://passportjs.org/).

This module lets you authenticate using Identity Server 4 OAuth 2.0 in your Node.js applications.
By plugging into Passport, OAuth 2.0 authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

Note that this strategy provides Identity Server 4 OAuth 2.0 support.  In many cases, a
provider-specific strategy can be used instead, which cuts down on unnecessary
configuration, and accommodates any provider-specific quirks.  

Developers who need to implement authentication against an OAuth 2.0 provider
that is not already supported are encouraged to sub-class this strategy.  If you
choose to open source the new provider-specific strategy, please add it to the
list so other people can find it.

## Install

    $ npm install passport-oauth2-ids4

## Usage

#### Configure Composer passport environment 

```js
export COMPOSER_PROVIDERS='{"oauth2": {"provider": "oauth2", "module": "passport-oauth2-ids4", "authorizationURL": "https://demo.identityserver.io/connect/authorize", "tokenURL": "https://demo.identityserver.io/connect/token", "userinfoURL": "https://demo.identityserver.io/connect/userinfo", "clientID": "server.code", "clientSecret": "secret", "scope": "openid profile email api offline_access", "authPath": "/auth/oauth2", "callbackURL": "/auth/oauth2/callback", "successRedirect": "http://hyperledger.eastasia.cloudapp.azure.com:4200?loggedIn=true", "failureRedirect": "/" } }'
```

#### Configure Strategy

The OAuth 2.0 authentication strategy authenticates users using a third-party
account and OAuth 2.0 tokens.  The provider's OAuth 2.0 endpoints, as well as
the client identifer and secret, are specified as options.  The strategy
requires a `verify` callback, which receives an access token and profile,
and calls `cb` providing a user.

```js
passport.use(new OAuth2Strategy({
    authorizationURL: 'https://demo.identityserver.io/connect/authorize',
    tokenURL: 'https://demo.identityserver.io/connect/token',
    userinfoURL: 'https://demo.identityserver.io/connect/userinfo',
    clientID: EXAMPLE_CLIENT_ID,
    clientSecret: EXAMPLE_CLIENT_SECRET,
    callbackURL: "https://www.example.net/auth/oauth2/callback"
  },
  function(accessToken, refreshToken, profile, cb) {
    User.findOrCreate({ exampleId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'oauth2'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.get('/auth/example',
  passport.authenticate('oauth2'));

app.get('/auth/example/callback',
  passport.authenticate('oauth2', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
```

## Related Modules

- [passport-oauth1](https://github.com/jaredhanson/passport-oauth1) — OAuth 1.0 authentication strategy
- [passport-http-bearer](https://github.com/jaredhanson/passport-http-bearer) — Bearer token authentication strategy for APIs
- [OAuth2orize](https://github.com/jaredhanson/oauth2orize) — OAuth 2.0 authorization server toolkit

## Contributing

#### Tests

The test suite is located in the `test/` directory.  All new features are
expected to have corresponding test cases.  Ensure that the complete test suite
passes by executing:

```bash
$ make test
```

#### Coverage

All new feature development is expected to have test coverage.  Patches that
increse test coverage are happily accepted.  Coverage reports can be viewed by
executing:

```bash
$ make test-cov
$ make view-cov
```

## Support

#### Funding

This software is provided to you as open source, free of charge.  The time and
effort to develop and maintain this project is dedicated by Applied Technology Group Limited.
If you (or your employer) benefit from this project, please consider a financial
contribution.  Your contribution helps continue the efforts that produce this
and other open source software.

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2018 Applied Technology Group Limited

