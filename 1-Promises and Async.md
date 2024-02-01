# Promises and Async/Await

_[back](./README.md)_

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

## Ye Olden Days

Javascript is highly asynchronous, with very few blocking operations. Node has functions like `fs.readSync()`, but those are often discouraged in favor of their async counterparts, as the asynchronicity is the secret of JS's speed since it is otherwise single-threaded. Classic JS async code was based on callbacks:

```js
function someAsyncOperation(arg, callback) {
    const result = arg + 5;

    // value is returned as an argument to the callback argument
    setTimeout(() => callback(result), 1000);
}

someAsyncOperation(7, result => {
    console.log(result);
})

// ew
someAsyncOperation(10, result => {
    someAsyncOperation(result, r => {
        console.log(r);
    });
});

console.log('after everything!'); // nope!
```

## Promises, Promises

Eventually, someone created the "Promise" concept to encapsulate async code in a slightly more linear-looking style, taking advantage of "method chaining", which was becoming popular at the time:

```js
function someAsyncOperation(arg) {
    // resolved value can be a return value, or another promise that eventually returns something else
    return new Promise((resolve, reject) => resolve(arg + 5));
}

someAsyncOperation(7)
    .then(result => console.log(result));

someAsyncOperation(10)
    // return value of then callback is passed to next then (after waiting, if it's a promise)
    .then(result => someAsyncOperation(result))
    .then(result => console.log(result));

console.log('after everything!'); // oops, this still doesn't work
```

Promises also had the "reject" path to deal with errors:

```js
function failOnFive(arg) {
    if (arg === 5)
        throw 'ERR: got five!';
    else
        return arg + 5;
}

try
{
    console.log(failOnFive(5));
}
catch (err)
{
    console.error(err);
}
finally
{
    console.log('done!');
}
```

becomes:

```js
function failOnFivePromise(arg) {
    return new Promise((resolve, reject) => {
        if (arg === 5)
            reject('ERR: got five!');
        else
            resolve(arg + 5);
    })
}

failOnFivePromise(5)
    .then(result => console.log(result))
    .catch(err => console.error(err))
    .finally(() => console.log('done!'));

// catch can send value back into the then path
failOnFivePromise(5)
    .catch(err => { console.error(err); return 7; })
    .then(result => console.log(result))
    .finally(() => console.log('done!'));

```

## Wait For Me

Promises became popular, but there was still room for improvement. The `async` and `await` keywords were added as syntax sugar around the now-standardized Promises:

```js
// same as Promises example
function someAsyncOperation(arg) {
    return new Promise((resolve, reject) => resolve(arg + 5));
}

async function DoOperationAsync() {
    // Promise-like things can be awaited in an async context
    console.log(await someAsyncOperation(5));
}

// literally compiles internally to something like this:
// function DoOperationAsync() {
//     return someAsyncOperation(5)
//         .then(result => console.log(result));
// }

DoOperationAsync().then(console.log('done!'));
```

One nice thing with async/await is that it works nicely with existing language structures like try/catch:

```js
async function failOnFiveAsync(arg) {
    if (arg === 5)
        throw 'got five!';
    else
        return arg + 5;
}

try {
    console.log(await failOnFiveAsync(5));
}
catch (err) {
    console.error(err);
}
finally {
    console.log('done!');
}
```