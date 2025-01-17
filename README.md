# passport-loopback3

Loopback 3 Authentication strategy for [Passport](http://passportjs.org/), based on `http-bearer`.

Loopback 3's authentication model is very close to, but not exactly the same as, HTTP bearer tokens, as
specified by [RFC 6750](http://tools.ietf.org/html/rfc6750). Specifically, while RFC 6750 specifies the authentication heading to be:

`Authorization: Bearer $TOKEN`

Loopback omits the Bearer keyword, and so the header would look like this:

`Authorization: $TOKEN`

Loopback also supports `access_token` URL parameters in exactly the same way HTTP Bearer.

This strategy supports both Loopback and RFC 6750 style authentication headers.

By plugging into Passport, bearer token support can be easily and unobtrusively
integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Usage

#### Configure Strategy

The HTTP Bearer authentication strategy authenticates users using a bearer
token.  The strategy requires a `verify` callback, which accepts that
credential and calls `done` providing a user.  Optional `info` can be passed,
typically including associated scope, which will be set by Passport at
`req.authInfo` to be used by later middleware for authorization and access
control.

```js
passport.use(new BearerStrategy(
  function(token, done) {
    User.findOne({ token: token }, function (err, user) {
      if (err) { return done(err); }
      if (!user) { return done(null, false); }
      return done(null, user, { scope: 'all' });
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'bearer'` strategy, to
authenticate requests.  Requests containing bearer tokens do not require session
support, so the `session` option can be set to `false`.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.get('/profile', 
  passport.authenticate('bearer', { session: false }),
  function(req, res) {
    res.json(req.user);
  });
```

#### Issuing Tokens

Bearer tokens are typically issued using OAuth 2.0.  [OAuth2orize](https://github.com/jaredhanson/oauth2orize)
is a toolkit for implementing OAuth 2.0 servers and issuing bearer tokens.  Once
issued, this module can be used to authenticate tokens as described above.

#### Making authenticated requests
The HTTP Bearer authentication strategy authenticates requests based on a bearer token contained in the:
* `Authorization` header field where the value is in the format `{scheme} {token}` and scheme is "Bearer" in this case.
* or `access_token` body parameter
* or `access_token` query parameter

## Examples

For a complete, working example, refer to the [Bearer example](https://github.com/passport/express-4.x-http-bearer-example).

## Related Modules

- [OAuth2orize](https://github.com/jaredhanson/oauth2orize) — OAuth 2.0 authorization server toolkit

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2013 Jared Hanson <[http://jaredhanson.net/](http://jaredhanson.net/)>
