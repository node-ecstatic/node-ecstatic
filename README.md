# Extatic [![build status](https://travis-ci.org/node-extatic/extatic.svg?branch=master)](http://travis-ci.org/node-extatic/extatic) [![codecov.io](https://codecov.io/github/node-extatic/extatic/coverage.svg?branch=master)](https://codecov.io/gh/node-extatic/extatic/)

![](http://imgur.com/vhub5.png)

A simple static file server middleware. Use it with a raw http server,
express/connect or on the CLI!

# Examples:

## express 4.x

``` js
'use strict';

const express = require('express');
const ecstatic = require('../lib/ecstatic');
const http = require('http');

const app = express();

app.use(ecstatic({
  root: `${__dirname}/public`,
  showdir: true,
}));

http.createServer(app).listen(8080);

console.log('Listening on :8080');
```

## stock http server

``` js
'use strict';

const http = require('http');

const ecstatic = require('../lib/ecstatic')({
  root: `${__dirname}/public`,
  showDir: true,
  autoIndex: true,
});

http.createServer(ecstatic).listen(8080);

console.log('Listening on :8080');
```

### fall through
To allow fall through to your custom routes:

```js
ecstatic({ root: __dirname + '/public', handleError: false })
```

## CLI

```sh
extatic ./public --port 8080
```

# Install:

For using extatic as a library, just npm install it into your project:

```sh
npm install --save extatic
```

For using extatic as a cli tool, either npm install it globally:

```sh
npm install extatic -g
```

or install it locally and use npm runscripts to add it to your $PATH, or
reference it directly with `./node_modules/.bin/extatic`.


# API:

## ecstatic(opts);
## $ ecstatic [dir?] {options} --port PORT

In node, pass ecstatic an options hash, and it will return your middleware!

```js
const opts = {
  root: path.join(__dirname, 'public'),
  baseDir: '/',
  autoIndex: true,
  showDir: true,
  showDotfiles: true,
  humanReadable: true,
  hidePermissions: false,
  si: false,
  cache: 'max-age=3600',
  cors: false,
  gzip: true,
  brotli: false,
  defaultExt: 'html',
  handleError: true,
  serverHeader: true,
  contentType: 'application/octet-stream',
  weakEtags: true,
  weakCompare: true,
  handleOptionsMethod: false,
}
```

If `opts` is a string, the string is assigned to the root folder and all other
options are set to their defaults.

When running in CLI mode, all options work as above, passed in
[optimist](https://github.com/substack/node-optimist) style. `port` defaults to
`8000`. If a `dir` or `--root dir` argument is not passed, ecsatic will
serve the current dir. Extatic also respects the PORT environment variable.

### `opts.root`
### `--root {root}`

`opts.root` is the directory you want to serve up.

### `opts.host`
### `--host {host}`

In CLI mode, `opts.host` is the host you want ecstatic to listen to. Defaults
to 0.0.0.0. This can be overridden with the `--host` flag or with the HOST
environment variable.

### `opts.port`
### `--port {port}`

In CLI mode, `opts.port` is the port you want ecstatic to listen to. Defaults
to 8000. This can be overridden with the `--port` flag or with the PORT
environment variable.

### `opts.baseDir`
### `--baseDir {dir}`

`opts.baseDir` is `/` by default, but can be changed to allow your static files
to be served off a specific route. For example, if `opts.baseDir === "blog"`
and `opts.root = "./public"`, requests for `localhost:8080/blog/index.html` will
resolve to `./public/index.html`.

### `opts.cache`
### `--cache {value}`

Customize cache control with `opts.cache` , if it is a number then it will set max-age in seconds.
Other wise it will pass through directly to cache-control. Time defaults to 3600 s (ie, 1 hour).

If it is a function, it will be executed on every request, and passed the pathname.  Whatever it returns, string or number, will be used as the cache control header like above.

### `opts.showDir`
### `--no-showDir`

Turn **off** directory listings with `opts.showDir === false`. Defaults to **true**.

### `opts.showDotfiles`
### `--no-showDotfiles`

Exclude dotfiles from directory listings with `opts.showDotfiles === false`. Defaults to **true**.

### `opts.humanReadable`
### `--no-human-readable`

If showDir is enabled, add human-readable file sizes. Defaults to **true**.
Aliases are `humanreadable` and `human-readable`.

### `opts.hidePermissions`
### `--hide-permissions`
If hidePermissions is enabled, file permissions will not be displayed. Defaults to **false**.
Aliases are `hidepermissions` and `hide-permissions`.

### `opts.headers`
### `--H {HeaderA: valA} [--H {HeaderB: valB}]`

Set headers on every response. `opts.headers` can be an object mapping string
header names to string header values, a colon (:) separated string, or an array
of colon separated strings.

`opts.H` and `opts.header` are aliased to `opts.headers` so that you can use
`-H` and `--header` options to set headers on the command-line like curl:

``` sh
$ ecstatic ./public -p 5000 -H 'Access-Control-Allow-Origin: *'
```

### `opts.si`
### `--si`

If showDir and humanReadable are enabled, print file sizes with base 1000 instead
of base 1024. Name is inferred from cli options for `ls`. Aliased to `index`, the
equivalent option in Apache.

### `opts.autoIndex`
### `--no-autoindex`

Serve `/path/index.html` when `/path/` is requested.
Turn **off** autoIndexing with `opts.autoIndex === false`. Defaults to **true**.

### `opts.defaultExt`
### `--defaultExt {ext}`

Turn on default file extensions with `opts.defaultExt`. If `opts.defaultExt` is
true, it will default to `html`. For example if you want a request to `/a-file`
to resolve to `./public/a-file.html`, set this to `true`. If you want
`/a-file` to resolve to `./public/a-file.json` instead, set `opts.defaultExt` to
`json`.

### `opts.gzip`
### `--no-gzip`

By default, ecstatic will serve `./public/some-file.js.gz` in place of
`./public/some-file.js` when the gzipped version exists and ecstatic determines
that the behavior is appropriate. If `./public/some-file.js.gz` is not valid
gzip, this will fall back to `./public/some-file.js`. You can turn this off
with `opts.gzip === false`.

### `opts.brotli`
### `--brotli`

Serve `./public/some-file.js.br` in place of `./public/some-file.js` when the 
[brotli encoded](https://github.com/google/brotli) version exists and ecstatic 
determines that the behavior is appropriate. If the request does not contain 
`br` in the HTTP `accept-encoding` header, ecstatic will instead attempt to 
serve a gzipped version (if `opts.gzip` is `true`), or fall back to 
`./public.some-file.js`. Defaults to **false**.

### `opts.serverHeader`
### `--no-server-header`

Set `opts.serverHeader` to false in order to turn off setting the `Server`
header on all responses served by ecstatic.

### `opts.contentType`
### `--content-type {type}`

Set `opts.contentType` in order to change default Content-Type header value.
Defaults to **application/octet-stream**.

### `opts.mimeTypes`
### `--mime-types {filename}`

Add new or override one or more mime-types. This affects the HTTP Content-Type
header. May be either an object hash of type(s), *or* a function
`(file, defaultValue) => 'some/mimetype'`. Naturally, only the object hash
works on the command line.

    ecstatic({ mimeTypes: { 'some/mimetype': ['file_extension', 'file_extension'] } })

It's important to note that any changes to mime handling are **global**, since
the `mime` module appears to be poorly behaved outside of a global singleton
context. For clarity you may prefer to call `require('ecstatic').mime.define`
or `require('ecstatic').setCustomGetType` directly and skip using this option,
particularly in cases where you're using multiple instances of ecstatic's
middleware. **You've been warned!**

### `opts.handleError`

Turn **off** handleErrors to allow fall-through with
`opts.handleError === false`, Defaults to **true**.

### `opts.weakEtags`
### `--no-weak-etags`

Set `opts.weakEtags` to false in order to generate strong etags instead of
weak etags. Defaults to **true**. See `opts.weakCompare` as well.

### `opts.weakCompare`
### `--no-weak-compare`

Turn off weakCompare to disable the weak comparison function for etag
validation. Defaults to **true**. See
https://www.ietf.org/rfc/rfc2616.txt Section 13.3.3 for more details.

### `opts.handleOptionsMethod`
### `--handle-options-method`

Set handleOptionsMethod to true in order to respond to 'OPTIONS' calls with any standard/set headers. Defaults to **false**. Useful for hacking up CORS support.

### `opts.cors`
### `--cors`

This is a **convenience** setting which turns on `handleOptionsMethod` and sets the headers **Access-Control-Allow-Origin: \*** and **Access-Control-Allow-Headers: Authorization, Content-Type, If-Match, If-Modified-Since, If-None-Match, If-Unmodified-Since**. This *should* be enough to quickly make cross-origin resource sharing work between development APIs. More advanced usage can come either from overriding these headers with the headers argument, or by using the `handleOptionsMethod` flag and then setting headers "manually." Alternately, just do it in your app using separate middlewares/abstractions.

Defaults to **false**.

## middleware(req, res, next);

This works more or less as you'd expect.

### ecstatic.showDir(folder);

This returns another middleware which will attempt to show a directory view. Turning on auto-indexing is roughly equivalent to adding this middleware after an ecstatic middleware with autoindexing disabled.

### ecstatic.mime.define(mappings);

This defines new mappings for the mime singleton, as specified in the main
docs for the ecstatic middleware. Calling this directly should make global
mutation more clear than setting the options when instantiating the middleware,
and is recommended if you're using more than one middlware instance.

### ecstatic.mime.setCustomGetType(fn);

This sets a global custom function for getting the mime type for a filename.
If this function returns a falsey value, getType will fall back to the mime
module's handling. Calling this directly should make global mutation more clear
than setting the options when instantiating the middleware, and is recommended
if you're using more than one middleware instance.

### ecstatic.mime.getType(filename, defaultValue);

This will return the mimetype for a filename, first using any function supplied
with `ecstatic.mime.setCustomGetType`, then trying `require('mime').getType`,
then falling back to defaultValue. Generally you don't want to use this
directly.

### ecstatic.mime.setCustomLookupCharset(fn);

This sets a global custom function for getting the charset for a mime type.
If this function returns a falsey value, lookupCharset will fall back on the
charset module's handling. Calling this directly should make global mutation
more clear than setting the options when instantiating the middleware, and is
recommended if you're using more than one middleware instance.

### ecstatic.mime.lookupCharset(mimeType);

This will look up the charset for the supplied mime type, first using any
function supplied with `ecstatic.mime.setCustomLookupCharset`, then trying
`require('charset')(mimeType)`. Generally you
don't want to use this directly.


# Tests:

Extatic has a fairly extensive test suite. You can run it with:

```sh
$ npm test
```

# Contribute:

Without outside contributions, ecstatic would wither and die! Before
contributing, take a quick look at the contributing guidelines in
[./CONTRIBUTING.md](./CONTRIBUTING.md) . They're relatively painless, I promise.

# License:

MIT. See LICENSE.txt. For contributors, see CONTRIBUTORS.md
