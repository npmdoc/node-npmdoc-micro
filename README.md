# api documentation for  [micro (v7.3.0)](https://github.com/zeit/micro#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-micro.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-micro) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-micro.svg)](https://travis-ci.org/npmdoc/node-npmdoc-micro)
#### Asynchronous HTTP microservices

[![NPM](https://nodei.co/npm/micro.png?downloads=true)](https://www.npmjs.com/package/micro)

[![apidoc](https://npmdoc.github.io/node-npmdoc-micro/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-micro_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-micro/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-micro/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-micro/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Zeit, Inc.",
        "email": "team@zeit.co"
    },
    "bin": {
        "micro": "./bin/micro.js"
    },
    "bugs": {
        "url": "https://github.com/zeit/micro/issues"
    },
    "dependencies": {
        "args": "2.4.0",
        "async-to-gen": "1.3.2",
        "bluebird": "3.5.0",
        "boxen": "1.0.0",
        "chalk": "1.1.3",
        "clipboardy": "1.0.2",
        "detect-port": "1.1.1",
        "ip": "1.1.5",
        "is-async-supported": "1.2.0",
        "isstream": "0.1.2",
        "media-typer": "0.3.0",
        "node-version": "1.0.0",
        "raw-body": "2.2.0",
        "update-notifier": "2.1.0"
    },
    "description": "Asynchronous HTTP microservices",
    "devDependencies": {
        "ava": "0.18.2",
        "coveralls": "2.12.0",
        "eslint-config-prettier": "1.5.0",
        "husky": "0.13.3",
        "lint-staged": "3.4.0",
        "nyc": "10.1.2",
        "prettier": "0.22.0",
        "request": "2.81.0",
        "request-promise": "4.2.0",
        "resumer": "0.0.0",
        "test-listen": "1.0.1",
        "then-sleep": "1.0.1",
        "xo": "0.18.1"
    },
    "directories": {},
    "dist": {
        "shasum": "31e6174ae44968964a770724f2ce488e99a8a652",
        "tarball": "https://registry.npmjs.org/micro/-/micro-7.3.0.tgz"
    },
    "files": [
        "bin",
        "lib"
    ],
    "gitHead": "40107f45f5a1233a2ca71accea324d4a1a2108d9",
    "homepage": "https://github.com/zeit/micro#readme",
    "keywords": [
        "micro",
        "service",
        "microservice",
        "serverless",
        "API"
    ],
    "license": "MIT",
    "lint-staged": {
        "*.js": [
            "npm run lint",
            "prettier --single-quote --write",
            "git add"
        ]
    },
    "main": "./lib/server.js",
    "maintainers": [
        {
            "name": "leo",
            "email": "mindrun@icloud.com"
        },
        {
            "name": "rauchg",
            "email": "rauchg@gmail.com"
        }
    ],
    "name": "micro",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/zeit/micro.git"
    },
    "scripts": {
        "lint": "xo",
        "precommit": "lint-staged",
        "test": "npm run lint && NODE_ENV=test nyc ava"
    },
    "version": "7.3.0",
    "xo": {
        "ignores": [
            "examples/**/*"
        ],
        "extends": "prettier"
    }
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module micro](#apidoc.module.micro)
1.  [function <span class="apidocSignatureSpan">micro.</span>buffer (req, { limit = '1mb', encoding } = {})](#apidoc.element.micro.buffer)
1.  [function <span class="apidocSignatureSpan">micro.</span>createError (code, msg, orig)](#apidoc.element.micro.createError)
1.  [function <span class="apidocSignatureSpan">micro.</span>json (req, opts)](#apidoc.element.micro.json)
1.  [function <span class="apidocSignatureSpan">micro.</span>run (req, res, fn)](#apidoc.element.micro.run)
1.  [function <span class="apidocSignatureSpan">micro.</span>send (res, code, obj = null)](#apidoc.element.micro.send)
1.  [function <span class="apidocSignatureSpan">micro.</span>sendError (req, res, { statusCode, status, message, stack })](#apidoc.element.micro.sendError)
1.  [function <span class="apidocSignatureSpan">micro.</span>text (req, { limit, encoding } = {})](#apidoc.element.micro.text)



# <a name="apidoc.module.micro"></a>[module micro](#apidoc.module.micro)

#### <a name="apidoc.element.micro.buffer"></a>[function <span class="apidocSignatureSpan">micro.</span>buffer (req, { limit = '1mb', encoding } = {})](#apidoc.element.micro.buffer)
- description and source-code
```javascript
(req, { limit = '1mb', encoding } = {}) =>
Promise.resolve().then(() => {
  const type = req.headers['content-type'] || 'text/plain';
  const length = req.headers['content-length'];
  encoding = encoding === undefined
    ? typer.parse(type).parameters.charset
    : encoding;

  const body = rawBodyMap.get(req);

  if (body) {
    return body;
  }

  return getRawBody(req, { limit, length, encoding })
    .then(buf => {
      rawBodyMap.set(req, buf);
      return buf;
    })
    .catch(err => {
      if (err.type === 'entity.too.large') {
        throw createError(413, 'Body exceeded ${limit} limit', err);
      } else {
        throw createError(400, 'Invalid body', err);
      }
    });
})
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.micro.createError"></a>[function <span class="apidocSignatureSpan">micro.</span>createError (code, msg, orig)](#apidoc.element.micro.createError)
- description and source-code
```javascript
function createError(code, msg, orig) {
  const err = new Error(msg);
  err.statusCode = code;
  err.originalError = orig;
  return err;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.micro.json"></a>[function <span class="apidocSignatureSpan">micro.</span>json (req, opts)](#apidoc.element.micro.json)
- description and source-code
```javascript
(req, opts) =>
exports.text(req, opts).then(body => parseJSON(body))
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.micro.run"></a>[function <span class="apidocSignatureSpan">micro.</span>run (req, res, fn)](#apidoc.element.micro.run)
- description and source-code
```javascript
(req, res, fn) =>
  new Promise(resolve => resolve(fn(req, res)))
.then(val => {
  if (val === null) {
    send(res, 204, null);
    return;
  }

  // Send value if it is not undefined, otherwise assume res.end
  // will be called later
  if (undefined !== val) {
    send(res, res.statusCode || 200, val);
  }
})
.catch(err => sendError(req, res, err))
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.micro.send"></a>[function <span class="apidocSignatureSpan">micro.</span>send (res, code, obj = null)](#apidoc.element.micro.send)
- description and source-code
```javascript
function send(res, code, obj = null) {
  res.statusCode = code;

  if (obj === null) {
    res.end();
    return;
  }

  if (Buffer.isBuffer(obj)) {
    if (!res.getHeader('Content-Type')) {
      res.setHeader('Content-Type', 'application/octet-stream');
    }

    res.setHeader('Content-Length', obj.length);
    res.end(obj);
    return;
  }

  if (isStream(obj)) {
    if (!res.getHeader('Content-Type')) {
      res.setHeader('Content-Type', 'application/octet-stream');
    }

    obj.pipe(res);
    return;
  }

  let str = obj;

  if (typeof obj === 'object' || typeof obj === 'number') {
    // We stringify before setting the header
    // in case 'JSON.stringify' throws and a
    // 500 has to be sent instead

    // the 'JSON.stringify' call is split into
    // two cases as 'JSON.stringify' is optimized
    // in V8 if called with only one argument
    if (DEV) {
      str = JSON.stringify(obj, null, 2);
    } else {
      str = JSON.stringify(obj);
    }

    if (!res.getHeader('Content-Type')) {
      res.setHeader('Content-Type', 'application/json');
    }
  }

  res.setHeader('Content-Length', Buffer.byteLength(str));
  res.end(str);
}
```
- example usage
```shell
...
const micro = require('micro')
const test = require('ava')
const listen = require('test-listen')
const request = require('request-promise')

test('my endpoint', async t => {
const service = micro(async (req, res) => {
  micro.send(res, 200, {
    test: 'woot'
  })
})

const url = await listen(service)
const body = await request(url)
...
```

#### <a name="apidoc.element.micro.sendError"></a>[function <span class="apidocSignatureSpan">micro.</span>sendError (req, res, { statusCode, status, message, stack })](#apidoc.element.micro.sendError)
- description and source-code
```javascript
function sendError(req, res, { statusCode, status, message, stack }) {
  statusCode = statusCode || status;

  if (statusCode) {
    send(res, statusCode, DEV ? stack : message);
  } else {
    send(res, 500, DEV ? stack : 'Internal Server Error');
  }

  if (!TESTING) {
    console.error(stack);
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.micro.text"></a>[function <span class="apidocSignatureSpan">micro.</span>text (req, { limit, encoding } = {})](#apidoc.element.micro.text)
- description and source-code
```javascript
(req, { limit, encoding } = {}) =>
exports
  .buffer(req, { limit, encoding })
  .then(body => body.toString(encoding))
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
