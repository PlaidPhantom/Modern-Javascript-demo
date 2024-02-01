# New Promises Methods

_[back](./README.md)_

## Promise.allSettled() (ES2020)

Similar to `Promise.all()` but completes whether or not the given promises returned successfully or not.

```js
> async function promiseA() { return 1; }
> async function promiseB() { throw new Error('oops'); }

> await Promise.all([promiseA(), promiseB()])
Uncaught Error: oops
    at promiseB (REPL21:1:35)
    at REPL30:1:64

> await Promise.allSettled([promiseA(), promiseB()])
[
  { status: 'fulfilled', value: 1 },
  {
    status: 'rejected',
    reason: Error: oops
        at promiseB (REPL21:1:35)
        at [... stack trace continues ...]
  }
]
```

## Promise.any() (ES2021)

Returns the value of the first Promise to complete.

```js
> function sleep(ms) { return new Promise(resolve => setTimeout(resolve, ms)); }
> async function slowPromise() { await sleep(1000); return 'slow'; }
> async function fastPromise() { await sleep(500); return 'fast'; }

> await Promise.any([slowPromise(), fastPromise()])
'fast'
```
