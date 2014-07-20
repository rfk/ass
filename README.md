[![Build Status](https://travis-ci.org/lloyd/ass.png)](https://travis-ci.org/lloyd/ass)

<center>![What kind of ass?](donkey.png)</center>

## **Ass**: Cross Process Code Coverage

`ass` is a small node.js code coverage library with the following features:

* **dynamic instrumentation**: code is instrumented on the fly
* **minimal setup**: get code coverage reports with minimal impact on your project
* **multiple process support**: coverage data from multiple processes is aggregated into a single report
* **different report formats**: basic reporting functionality built in

## Theory

The philosophy behind `ass` is that implementing code coverage should
be a trivial process.  That no pre or post processing should be
required.  That the code coverage library should itself handle
reporting (not require support from your test framework).  And
finally, that code coverage data should be programatically accessible.

## Practice

To get started with ass, first install it:

    npm install --save-dev ass

Then instrument processes that are run by your test harness by adding a
single "stub" file (say your original server was in `server.js`, let's assume
you name the stub `stub.js`):

    require('ass');
    require('./server.js');

Finally, you can enable testing in your test harness programatically:

    var ass = require('ass').enable();

    // .. run all of your tests, spawning instrumented processes

    ass.report('html', function(err, report) {
      require('fs').writeFileSync('coverage.html', report);
    });

The `enable` function accepts an `options` hash. There are two supported settings:

* `exclude` - files to exclude from coverage - passed to `blanket` - see [Blanket Options](https://github.com/alex-seville/blanket/blob/master/docs/browser_options.md)

* `reporters` - a hash containing references to reporter implementations, e.g., to generate an SVG button. (`json` and `html` reporters are included with `ass`).

```
    var ass = require('ass').enable({
      exclude: [ './vendor' ],
      reporters: {
        svg: function(data, callback) {
          // ...
        }
      }
    });
```

## Example

A full example of code coverage is available:

    $ git clone git://github.com/lloyd/ass
    $ cd ass/example
    $ npm install
    $ npm test
    $ open coverage.html

## License

MIT

## Credits

The infamous [Zach Carter](http://github.com/zaach) created our fantastic logo.

The design of the html reporter was lifted from [TJ Holowaychuk](https://twitter.com/tjholowaychuk)'s fantastic
[mocha test framework](http://visionmedia.github.io/mocha/).
