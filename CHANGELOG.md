2019/06/15 Version 4.1.3
- fix redirections on Windows

2019/04/27 Version 4.1.2
- Redirects bugfix

2019/04/20 Version 4.1.1
- Respect handleError for 400 and 500 errors

2019/04/15 Version 4.1.0
- Add ability to set the host in cli mode

2019/04/14 Version 4.0.2
- Add dependency for is-finished library
- Fixes for edge cases around closed/finished streams

2019/04/12 Version 4.0.1
- Fix file descriptor leak from upstream response closing

2019/04/05 Version 4.0.0
- Drop testing/support for nodes 4 and 5, test nodes 9, 10 and 11
- Fix parsing of CORS options
- Upgrade mime module to v2, use charset module for charset detection
- Remove ability to set mime types with a .types file
- Add ability to override mime type and charset lookup with globally-set
  functions
- Removes default charset of utf8 - if you need this, try using a custom
  charset lookup function
- Move bin behavior from inside ./lib/ecstatic.js into ./lib/bin.js - see issue
  #226 for more information
- Update modules and fix linting

2019/02/10 Version 3.3.1
- Publish via linux to hopefully fix #238

2018/09/01 Version 3.3.0
- Updated dependencies
- Added support for brotli encoding in addition to existing gzip support

2018/08/28 Version 3.2.2
- Patchfix for improved accuracy when checking to see if gzip responses are allowed
- Updated tap and package-lock.json

2018/07/06 Version 3.2.1
- Update package.json project description
- Move tools folder to scripts folder
- Linting now passes on windows if git converts newlines to CRLFs
- No longer return a 416 when input range header has extra untrimmed whitespace
- Remove extra double quotes in ETAGs

2018/02/03 Version 3.2.0
- Add hidePermissions flag to hide file permissions from directory listings

2017/12/16 Version 3.1.1
- Requires version of node-mime that patches regexp dos vulnerability

2017/12/09 Version 3.1.0
- Minor tweaks/fixes to directory view
- Add a mess of dotfiles to .npmignore

2017/08/28 Version 3.0.0
- Lint ./lib/ ./example and ./test  against airbnb modified to support node 4.x
  and a few quirky hard-to-fix idioms
- Change gzip behavior to default
- Change weak etags and weak etag comparisons to be on by default
- Remove support for 0.12.0
- Remove union examples and test harnesses (support should have been removed
  long ago)
- Fix icon styles in directory listing for small screens
- Update mime to ^v1.4.0 - This changes gzip responses to always have application/gzip as their content-type

2017/06/06 Version 2.2.1
- Fix version number in CHANGELOG.md

2017/06/06 Version 2.2.0
- Will now properly serve gzip files when defaulting the extension
- Will fall back to serving non-gzip files if file with .gz extension is
  missing the magic bytes
- Updated he, url-join
- Updated devDependencies
- Added .npmrc
- Added package-lock.json
- Much improved documentation for the cli component

2016/08/10 Version 2.1.0
- New, prettier showDir pages with icons!

2016/08/09 Version 2.0.0
- No longer strip null bytes from uris before parsing. This avoids a regexp dos
  attack. The stripping was to avoid a bug regarding c++ null terminated
  strings shenanigans in some versions of node, but it *appears* fixed in LTS
  versions of node.
- When both showDir and autoIndex are turned off, do not redirect from /foo to
  /foo/.
- Add code coverage reports and codecov.io

2015/05/10 Version 1.4.1
- Compare if-modified-since header against server-generated last-modified
  header rather than raw mtime

2015/12/22 Version 1.4.0
- Add ability to specify custom mimetypes via a JSON blob (on the CLI)
- Started test suite around CLI options parsing
- Workaround for egregious v8 bug around date parsing throwing during
  modified-since checks

2015/11/15 Version 1.3.1
- Add recent contributors to CONTRIBUTORS.md
- Document showDotFiles in main options example

2015/11/14 Version 1.3.0
- opts.showDotFiles allows hiding dot files

2015/11/03 Version 1.2.0
- opts.cache supports function argument

2015/10/03 Version 1.1.3
- Add CORS=false to defaults

2015/10/02 Version 1.1.2
- Properly handle defaults in CLI args

2015/10/02 Version 1.1.1
- Properly handle boolean CLI args

2015/10/01 Version 1.1.0
- Adds support for responding to OPTIONS headers
- Adds support for setting custom headers
- Adds cors convenience setting

2015/09/22 Version 1.0.1
- Use encodeURIComponent when creating links in showdir

2015/09/14 Version 1.0.0
- Optional support for weak Etags and weak Etag *comparison*, useful for cases
  where one is running ecstatic with gzip behind an nginx proxy (these will
  likely be turned ON by default in a following major version)
- As a bin, respects process.env.PORT when binding to a port
- Directory listings encode pathnames, etc
- Default status pages return html instead of text/plain
- Contributors are listed in CONTRIBUTORS.md, referenced by LICENSE.txt

2015/05/22 Version 0.8.0
- Add ability to define custom mime-types, inline or with Apache .types file
- Test against express ^4.12.3 and union ^0.4.4
- Run tests with tap ^1.0.3
- Fix newline asserts to work with windows
- Add license attribute to package.json
- Elaborate contribution guidelines

2015/05/09 Version 0.7.6
- Fix double encoding in directory listings

2015/05/07 Version 0.7.5
- Fix HTML reflection vulnerability in certain error handlers

2015/04/17 Version 0.7.4
- Fix sort ordering in directory listings

2015/04/13 Version 0.7.3
- Close fstream if/when res closes, fixes potential fd leak

2015/04/05 Version 0.7.2
- Correctly handle req.statusCode in recursive calls; do not inherit upstream res.statusCode

2015/03/27 Version 0.7.1
- Treat ENOTDIR as 404 (same as ENOENT)

2015/03/18 Version 0.7.0
- Add support for specifying default content-type (as an alternative to application/octet-stream)
- Use url-join for autoIndex route, fixes windows problems

2015/03/01 Version 0.6.1
- Fix handleError fall-through with directory listings

2015/02/16 Version 0.6.0
- Fix for pathname decoding in windows
- Fix for hrefs in directory listings
- Add ability to turn off setting of Server header
- Remove extraneous call to res.end (handled by stream pipe)
- Remove tests from npm package due to jenkins bug
- Start a ChangeLog.md
