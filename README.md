# promise-all-sequential

[![NPM Version][npm-version]][npm-url]
[![License][license-badge]][license-url]

Extremely lightweight (270 bytes) library for evaluating promises sequentially with zero external dependencies.

```js
const promiseAllSequential = require("promise-all-sequential");

const asyncFunc1 = async () => new Promise((resolve) => {
    setTimeout(() => {
        console.log("First");
        resolve(1);
    }, 1000);
});

const asyncFunc2 = async () => new Promise((resolve) => {
    setTimeout(() => {
        console.log("Second");
        resolve(2);
    }, 250);
});

// Prints "First", then "Second" and returns results
promiseAllSequential([asyncFunc1, asyncFunc2]).then((result) => {
    console.log(result); // [1, 2]
});
```

Essentially, `asyncFunc1` is guaranteed to finish before `asyncFunc2` gets called. The result will be a promise which resolves with an array of results from the functions.

## Installation
This is a [Node.js](https://nodejs.org/en/) module available through the [npm registry](https://www.npmjs.com/).

Before installing, [download and install Node.js](https://nodejs.org/en/download/).

Installation is done using the [`npm install` command](https://docs.npmjs.com/getting-started/installing-npm-packages-locally):
```bash
$ npm install promise-all-sequential
```

## Examples

Comparison with [`Promise.all()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) that calls functions simultaneously:

```js
// Prints "Second", then "First" - wrong order!
Promise.all([asyncFunc1(), asyncFunc2()]).then((results) => {
    console.log(results); // [1, 2] - Correct
});
```

Comparison with chaining asynchronous functions:
```js
// Using async function chaining
const arr = [];
const results = async1().then((r1) => {
    arr.push(r1);
    return async2().then((r2) => {
        arr.push(r2);
        return async3().then((r3) => {
            arr.push(r3);
            return arr;
        });
    })
});
console.log(results);

// Using this library
promiseAllSequential([async1, async2, async3]).then((results) => {
    console.log(results);
});
```


## Troubleshooting

Please note that **you cannot initialize the promise directly**, meaning that the following will not work:
```js
const asyncFunc = new Promise(...);
```
This is because a promise starts executing its body as soon as it is created, making it no longer possible to force into sequential order. You must wrap the initialization into a function instead:
```js
const asyncFunc = () => new Promise(...);
```

If you face any issues, please submit an [issue](https://github.com/jhamberg/promise-all-sequential/issues) or a [pull request](https://github.com/jhamberg/promise-all-sequential/pulls).


## People
The author of promise-all-sequential is [Jonatan Hamberg](https://www.cs.helsinki.fi/u/hajo/)

[List of all contributors](https://github.com/jhamberg/promise-all-sequential/graphs/contributors)

## License
[MIT](LICENSE)

[npm-version]: https://img.shields.io/npm/v/promise-all-sequential.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/promise-all-sequential
[license-badge]: https://img.shields.io/badge/License-MIT-yellow.svg
[license-url]: https://opensource.org/licenses/MIT