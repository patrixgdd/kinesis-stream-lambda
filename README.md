kinesis-stream-lambda
=====================

[![NPM Version][npm-image]][npm-url]
[![Build Status](https://travis-ci.org/tilfin/kinesis-stream-lambda.svg?branch=master)](https://travis-ci.org/tilfin/kinesis-stream-lambda)

## How to install

```
$ npm install -save kinesis-stream-lambda
```

## Lambda handler example

```javascript
const es = require('event-stream');
const KSL = require('kinesis-stream-lambda');


exports.handler = function(event, context) {
  console.log('event: ', JSON.stringify(event, null, 2));

  const result = [];
  const stream = KSL.reader(event, { isAgg: false });

  stream.on('end', function() {
    context.done();
  })
  .pipe(KSL.parseJSON({ expandArray: false }))
  .pipe(es.map(function(data, callback) {
    result.push(data);
    callback(null, data)
  }))
  .on('end', function(){
    JSON.stringify(result, null, 2);
  });
}
```

## License

  [MIT](LICENSE)

[npm-image]: https://img.shields.io/npm/v/kinesis-stream-lambda.svg
[npm-url]: https://npmjs.org/package/kinesis-stream-lambda