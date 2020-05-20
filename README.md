
# Regarding denoify

- Changes made to [``package.json``](https://github.com/garronej/ts-md5/commit/3d72eced7f75da133ac75d23cec102d74f5b8ab2#diff-b9cfc7f2cdf78a7f4b91a753d10865a2)
- Changes made to [``tsconfig.json``](https://github.com/garronej/ts-md5/commit/3d72eced7f75da133ac75d23cec102d74f5b8ab2#diff-e5e546dd2eb0351f813d63d1b39dbc48)
- Some straight forward changes had to be made on the sources to comply with the stricter typescript directive, [checkout the ``src/*.ts`` files in the commit](https://github.com/garronej/ts-md5/commit/920536cb8756ca6a460e4abaea17f5e52e3fbefb)

Then
```bash
> npm run build
```
have been run witch has resulted in ``./mod.ts`` and ``./deno_dist`` being created.  
  
Finally a release named ``1.2.7`` have been created.  

Now it is possible to import ``"ts-md5"`` in deno:
```typescript
import { Md5 } from "https://raw.github.com/garronej/ts-md5/1.2.7/mod.ts";
```

And to use it as dependency in packages that uses denoify:

package.json
```json
{
     "dependencies": {
          "ts-md5": "garronej/ts-md5#1.2.7"
     }
}
```

( following is the original ``"ts-md5"`` readme )

# Introduction

A MD5 implementation for TypeScript

* Can handle Unicode strings
* Supports incremental hashing
* Works with Files and Blobs

This library also includes tools for:

* Hashing a file or blob
* A webworker for performing hashing
* A webworker handler for requesting files or blobs to be hashed
     * promise based
     * files or blobs are queued for processing on the webworker


Based on work by

* Joseph Myers: http://www.myersdaily.org/joseph/javascript/md5-text.html
* André Cruz: https://github.com/satazor/SparkMD5
* Raymond Hill: https://github.com/gorhill/yamd5.js


## Usage

### Basic Hashing

1. Import the class
     * `import {Md5} from 'ts-md5/dist/md5';`
2. Hash some things
     * `Md5.hashStr('blah blah blah')` => hex:string
     * `Md5.hashStr('blah blah blah', true)` => raw:Int32Array(4)
     * `Md5.hashAsciiStr('blah blah blah')` => hex:string
     * `Md5.hashAsciiStr('blah blah blah', true)` => raw:Int32Array(4)

For more complex uses:

```typescript

md5 = new Md5();

// Append incrementally your file or other input
// Methods are chainable
md5.appendStr('somestring')
    .appendAsciiStr('a different string')
    .appendByteArray(blob);

// Generate the MD5 hex string
md5.end();

```


### Hashing a File

NOTE:: You have to make sure `ts-md5/dist/md5_worker.js` is made available in your build so it can be accessed directly by a browser
It should always remain as a seperate file.

```typescript

import {ParallelHasher} from 'ts-md5/dist/parallel_hasher';

let hasher = new ParallelHasher('/path/to/ts-md5/dist/md5_worker.js');
hasher.hash(fileBlob).then(function(result) {
   console.log('md5 of fileBlob is', result);
});

```


## Building from src

The project is written in typescript and transpiled into ES5.

1. Install TypeScript: `npm install -g typescript` (if you haven't already)
2. Configure compile options in `tsconfig.json`
3. Perform build using: `tsc`

You can find more information here: https://github.com/Microsoft/TypeScript/wiki/tsconfig.json

## Type Definitions

These allow projects like jasmine to work with .ts files. This is here as a guide more than anything else.
See: https://github.com/typings/typings

1. Install typings: `npm install typings --global` (if you haven't already)
2. Download definitions: `typings install jasmine --ambient --save`
     * Note: ambient means we are searching in https://github.com/DefinitelyTyped/DefinitelyTyped
3. Reference the typings using `/// <reference path="../typings/main.d.ts" />` from tests

To update typings:

*  From command line run: `typings install`


## Scripts

1. Build Script: `npm run build`
2. Test Script: `npm run test`


## Publishing

1. Sign up to https://www.npmjs.com/
2. Configure `package.json` https://docs.npmjs.com/files/package.json
3. run `npm publish` https://docs.npmjs.com/cli/publish


# License

MIT
