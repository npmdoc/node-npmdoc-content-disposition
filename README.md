# api documentation for  [content-disposition (v0.5.2)](https://github.com/jshttp/content-disposition#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-content-disposition.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-content-disposition) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-content-disposition.svg)](https://travis-ci.org/npmdoc/node-npmdoc-content-disposition)
#### Create and parse Content-Disposition header

[![NPM](https://nodei.co/npm/content-disposition.png?downloads=true)](https://www.npmjs.com/package/content-disposition)

[![apidoc](https://npmdoc.github.io/node-npmdoc-content-disposition/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-content-disposition_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-content-disposition/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-content-disposition/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-content-disposition/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/jshttp/content-disposition/issues"
    },
    "contributors": [
        {
            "name": "Douglas Christopher Wilson",
            "email": "doug@somethingdoug.com"
        }
    ],
    "dependencies": {},
    "description": "Create and parse Content-Disposition header",
    "devDependencies": {
        "eslint": "3.11.1",
        "eslint-config-standard": "6.2.1",
        "eslint-plugin-promise": "3.3.0",
        "eslint-plugin-standard": "2.0.1",
        "istanbul": "0.4.5",
        "mocha": "1.21.5"
    },
    "directories": {},
    "dist": {
        "shasum": "0cf68bb9ddf5f2be7961c3a85178cb85dba78cb4",
        "tarball": "https://registry.npmjs.org/content-disposition/-/content-disposition-0.5.2.tgz"
    },
    "engines": {
        "node": ">= 0.6"
    },
    "files": [
        "LICENSE",
        "HISTORY.md",
        "README.md",
        "index.js"
    ],
    "gitHead": "2a08417377cf55678c9f870b305f3c6c088920f3",
    "homepage": "https://github.com/jshttp/content-disposition#readme",
    "keywords": [
        "content-disposition",
        "http",
        "rfc6266",
        "res"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "dougwilson",
            "email": "doug@somethingdoug.com"
        }
    ],
    "name": "content-disposition",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/jshttp/content-disposition.git"
    },
    "scripts": {
        "lint": "eslint .",
        "test": "mocha --reporter spec --bail --check-leaks test/",
        "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --reporter dot --check-leaks test/",
        "test-travis": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --reporter spec --check-leaks test/"
    },
    "version": "0.5.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module content-disposition](#apidoc.module.content-disposition)
1.  [function <span class="apidocSignatureSpan">content-disposition.</span>parse (string)](#apidoc.element.content-disposition.parse)



# <a name="apidoc.module.content-disposition"></a>[module content-disposition](#apidoc.module.content-disposition)

#### <a name="apidoc.element.content-disposition.parse"></a>[function <span class="apidocSignatureSpan">content-disposition.</span>parse (string)](#apidoc.element.content-disposition.parse)
- description and source-code
```javascript
function parse(string) {
  if (!string || typeof string !== 'string') {
    throw new TypeError('argument string is required')
  }

  var match = DISPOSITION_TYPE_REGEXP.exec(string)

  if (!match) {
    throw new TypeError('invalid type format')
  }

  // normalize type
  var index = match[0].length
  var type = match[1].toLowerCase()

  var key
  var names = []
  var params = {}
  var value

  // calculate index to start at
  index = PARAM_REGEXP.lastIndex = match[0].substr(-1) === ';'
    ? index - 1
    : index

  // match parameters
  while ((match = PARAM_REGEXP.exec(string))) {
    if (match.index !== index) {
      throw new TypeError('invalid parameter format')
    }

    index += match[0].length
    key = match[1].toLowerCase()
    value = match[2]

    if (names.indexOf(key) !== -1) {
      throw new TypeError('invalid duplicate parameter')
    }

    names.push(key)

    if (key.indexOf('*') + 1 === key.length) {
      // decode extended value
      key = key.slice(0, -1)
      value = decodefield(value)

      // overwrite existing value
      params[key] = value
      continue
    }

    if (typeof params[key] === 'string') {
      continue
    }

    if (value[0] === '"') {
      // remove quotes and escapes
      value = value
        .substr(1, value.length - 2)
        .replace(QESC_REGEXP, '$1')
    }

    params[key] = value
  }

  if (index !== -1 && index !== string.length) {
    throw new TypeError('invalid parameter format')
  }

  return new ContentDisposition(type, params)
}
```
- example usage
```shell
...
##### type

Specifies the disposition type, defaults to '"attachment"'. This can also be
'"inline"', or any other value (all values except inline are treated like
'attachment', but can convey additional information if both parties agree to
it). The type is normalized to lower-case.

### contentDisposition.parse(string)

'''js
var disposition = contentDisposition.parse('attachment; filename="EURO rates.txt"; filename*=UTF-8\'\'%e2%82%ac%20rates.txt');
'''

Parse a 'Content-Disposition' header string. This automatically handles extended
("Unicode") parameters by decoding them and providing them under the standard
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
