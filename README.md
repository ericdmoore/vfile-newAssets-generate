# vfile-newAssets-generate
Works with `rehype-all-the-thumbs` and others to generate the new derived assets based an input file

## Overview

_Configuration_:
- write file closure?
  - `{ destName: <string>, write: async (s) => Promise<boolean> }`

_Input_:
- a UNIST tree of some kind
- vfile with added `newAssets` key added to the object

_Output_:
- unchanged UNIST tree 
- the given write function is called on each vfile in the newAssets array.
- then `vfile.{{destName}}` is an `{ inputCount: N, outputCount:N, assetsWritten: vfile[] }`


## Ships With:

### gen function

_Usage:_

1. fs usage
```js
const fs = require('fs')
const {gen} = require('vfile-newAssets-generate')
const write = gen(fs)
```

2. S3 usage
```js
const aws = require('aws-sdk')
const {gen} = require('vfile-newAssets-generate')
const s3c = new aws.S3({...})
const write = gen(s3c)
```

### Also Allows BYO Logic

3. Custom Writer Logic

```js
const writer = { 
  write: (vf, i , arr, next) => {
    // `i` is the index of where it is the filtered newAssets array
    // `arr` is an Object.frozen filtered newAssets array
    // do stuff to save the vfile to the network
    console.log('save to console')
    console.log(vf)
    cb(null)
  } 
}

/* 
  type cb = (Error, dataFromDestination: any ) => void
*/

```
