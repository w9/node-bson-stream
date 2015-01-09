# BSONStream

Read a binary stream that contains BSON objects and emit each as a JavaScript
object.


## Example

Write each object of a BSON file created with mongodump to stdout:

    var fs = require('fs');
    var BSONStream = require('bson-stream');

    var rs = fs.createReadStream('/some/data.bson');

    rs.pipe(new BSONStream()).on('data', function(obj) {
      console.log(obj);
    });

Example with an object containing a Buffer object as a value. The Buffer will be
embedded when serialized by the BSON npm:

    var assert = require('assert');
    var BSON = require('bson').BSONPure.BSON;
    var BSONStream = require('bson-stream');

    var buf = new Buffer([65, 67, 70]); // 'ACF'
    var obj = { foo: buf };

    var bs = new BSONStream();
    bs.on('data', function(obj) {
      // the original Buffer value is embedded in a new BSON Buffer object and
      // stored at the `buffer` key
      assert.strictEqual(obj.foo.buffer.toString(), 'ACF');
    });

    bs.end(BSON.serialize(obj));


## Installation

    $ npm install bson-stream


## API

### BSONStream([opts])
* [opts] {Object} object containing optional parameters

opts:
* raw {Boolean, default false} whether to emit JavaScript objects or raw Buffers
* maxDocLength {Number, default 16777216} maximum BSON document size in bytes
* maxBytes {Number, default infinite} maximum number of bytes to receive
* debug {Boolean, default false} whether to do extra console logging or not
* hide {Boolean, default false} whether to suppress errors or not (used in tests)

Read a binary stream that contains BSON objects and emit each as a JavaScript
object (or as a Buffer if opts.raw is true).

Note: implements BSON specification 1.0 (http://bsonspec.org/spec.html), as
supported by js-bson (https://www.npmjs.org/package/bson).

Note2: Buffer objects will be embedded in a new BSON Buffer object at `buffer`.


## Tests

    $ npm test


# License

MIT

Copyright (c) 2014 Tim Kuijsten

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
