# exit [![Build Status](https://secure.travis-ci.org/cowboy/node-exit.png?branch=master)](http://travis-ci.org/cowboy/node-exit)

A replacement for process.exit that ensures stdio are fully drained before exiting.

To make a long story short, if `process.exit` is called on Windows, script output is often truncated when pipe-redirecting `stdout` or `stderr`. This module attempts to work around this issue by waiting until those streams have been completely drained before actually calling `process.exit`.

See [Node.js issue #3584](https://github.com/joyent/node/issues/3584) for further reference.

Tested in OS X 10.8, Windows 7 on Node.js 0.8.25 and 0.10.18.

Based on some code by [@vladikoff](https://github.com/vladikoff).

## Getting Started
Install the module with: `npm install exit`

```javascript
var exit = require('exit');

// These lines should appear in the output, EVEN ON WINDOWS.
console.log("foo");
console.error("bar");

// process.exit(5);
exit(5);

// These lines shouldn't appear in the output.
console.log("foo");
console.error("bar");
```

## Don't believe me? Try it for yourself.

In Windows, clone the repo and cd to the `test\fixtures` directory. The only difference between [log.js](test/fixtures/log.js) and [log-broken.js](test/fixtures/log-broken.js) is that the former uses `exit` while the latter calls `process.exit` directly.

```
C:\node-exit\test\fixtures>node log.js 0 10 stdout stderr 2>&1 | find "std"
[stdout] testing 0
[stderr] testing 0
[stdout] testing 1
[stderr] testing 1
[stdout] testing 2
[stderr] testing 2
[stdout] testing 3
[stderr] testing 3
[stdout] testing 4
[stderr] testing 4
[stdout] testing 5
[stderr] testing 5
[stdout] testing 6
[stderr] testing 6
[stdout] testing 7
[stderr] testing 7
[stdout] testing 8
[stderr] testing 8
[stdout] testing 9
[stderr] testing 9

C:\node-exit\test\fixtures>node log-broken.js 0 10 stdout stderr 2>&1 | find "std"

C:\node-exit\test\fixtures>
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
2013-09-20 - v0.1.0 - Initial release.

## License
Copyright (c) 2013 "Cowboy" Ben Alman  
Licensed under the MIT license.
