# Other Features

_[back](./README.md)_

## Object.fromEntries() (ES2019)

Basically the reverse of `Object.entries()`, return all key-value pairs from an object as an array.

```js
> Object.fromEntries([ [ 'a', 1 ], [ 'b', 2 ], [ 'c', 3 ] ])
{ a: 1, b: 2, c: 3 }
```

## Null coalescing operators (ES2020)

Basically the same as in C#, will catch null and undefined.

```js
> 5 ?? 6
5
> null ?? 6
6
> null?.a
undefined
```

## Numeric Separators (ES2021)

Numbers can now have underscores to help visualize.

```js
> 1_000_000
1000000
```

## Class fields and private fields (ES2022)

Previously, class-level fields were only in Typescript, not Javascript.

```js
> class MyClass {
    myprop = 5;

    constructor(val) {
        this.myprop + val;
    }
}

> const c = new MyClass(4);
> c.myprop
5
```

```js
> class MyClass {
    #myprop = 5;

    constructor(val) {
        this.#myprop + val;
    }
}

> const c = new MyClass(4);
> c.#myprop;
c.#myprop
 ^

Uncaught SyntaxError: Private field '#myprop' must be declared in an enclosing class
```

