# Changes

This file is a manually maintained list of changes for each release. Feel free
to add your changes here when sending pull requests. Also send corrections if
you spot any mistakes.

## v2.0.0-alpha

This release is a rewrite. You should carefully test your application after
upgrading to avoid problems. This release features many improvements, most
importantly:

* ~5x faster than v0.9.x for parsing query results
* Support for pause() / resume() (for streaming rows)
* Support for multiple statement queries
* Support for stored procedures
* Support for transactions
* Support for binary columns (as blobs)
* Consistent & well documented error handling
* A new Connection class that has well defined semantics (unlike the old Client class).
* Convenient escaping of objects / arrays that allows for simpler query construction
* A significantly simpler code base
* Many bug fixes & other small improvements (Closed 62 out of 66 GitHub issues)

Below are a few notes on the upgrade process itself:

The first thing you will run into is that the old `Client` class is gone and
has been replaced with a less ambitious `Connection` class. So instead of
`mysql.createClient()`, you now have to:

```js
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
});

connection.query('SELECT 1', function(err, rows) {
  if (err) throw err;

  console.log('Query result: ', rows);
});

connection.end();
```

The new `Connection` class does not try to handle re-connects, please study the
`Server disconnects` section in the new Readme.

Other than that, the interface has stayed very similar. Here are a few things
to check out so:

* BIGINT's are now cast into strings
* Binary data is now cast to buffers
* The `'row'` event on the `Query` object is now called `'result'` and will
  also be emitted for queries that produce an OK/Error response.
* Error handling is consistently defined now, check the Readme
* Escaping has become more powerful which may break your code if you are
  currently using objects to fill query placeholders.
* Connections can now be established explicitly again, so you may wish to do so
  if you want to handle connection errors specifically.

That should be most of it, if you run into anything else, please send a patch
or open an issue to improve this document.

## v0.9.6

* Escape array values so they produce sql arrays (Roger Castells, Colin Smith)
* docs: mention mysql transaction stop gap solution (Blake Miner)
* docs: Mention affectedRows in FAQ (Michael Baldwin)

## v0.9.5

* Fix #142 Driver stalls upon reconnect attempt that's immediately closed
* Add travis build
* Switch to urun as a test runner
* Switch to utest for unit tests
* Remove fast-or-slow dependency for tests
* Split integration tests into individual files again

## v0.9.4

* Expose package.json as `mysql.PACKAGE` (#104)

## v0.9.3

* Set default `client.user` to root
* Fix #91: Client#format should not mutate params array
* Fix #94: TypeError in client.js
* Parse decimals as string (vadimg)

## v0.9.2

* The underlaying socket connection is now managed implicitly rather than explicitly.
* Check the [upgrading guide][] for a full list of changes.

## v0.9.1

* Fix issue #49 / `client.escape()` throwing exceptions on objects. (Nick Payne)
* Drop < v0.4.x compatibility. From now on you need node v0.4.x to use this module.

## Older releases

These releases were done before maintaining this file:

* [v0.9.0](https://github.com/felixge/node-mysql/compare/v0.8.0...v0.9.0)
* [v0.8.0](https://github.com/felixge/node-mysql/compare/v0.7.0...v0.8.0)
* [v0.7.0](https://github.com/felixge/node-mysql/compare/v0.6.0...v0.7.0)
* [v0.6.0](https://github.com/felixge/node-mysql/compare/v0.5.0...v0.6.0)
* [v0.5.0](https://github.com/felixge/node-mysql/compare/v0.4.0...v0.5.0)
* [v0.4.0](https://github.com/felixge/node-mysql/compare/v0.3.0...v0.4.0)
* [v0.3.0](https://github.com/felixge/node-mysql/compare/v0.2.0...v0.3.0)
* [v0.2.0](https://github.com/felixge/node-mysql/compare/v0.1.0...v0.2.0)
* [v0.1.0](https://github.com/felixge/node-mysql/commits/v0.1.0)