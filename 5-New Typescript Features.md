# New Typescript Features

_[back](./README.md)_

## Auto-accessors (4.9)

https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#auto-accessors-in-classes

Accessors are like C# properties, a getter and setter with a hidden private field. They've been added to TS and JS for technical reasons

```ts
class MyClass {
    accessor myprop;

    constructor() {
        this.myprop = 5;
    }
}
```

transpiles to (in ES2022, varies with other targets):

```ts
class MyClass {
    #myprop_accessor_storage;
    get myprop() { return this.#myprop_accessor_storage; }
    set myprop(value) { this.#myprop_accessor_storage = value; }
    constructor() {
        this.myprop = 5;
    }
}
```

## Non-experimental decorators

https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html

Typescript has had "experimental" decorators based on early drafts of the JS Decorators proposal. Now the proposal is closer to finalization, and Typescript has updated Decorators to match the current proposal.

```ts
function loggedMethod(originalMethod: any, _context: any) {
    function replacementMethod(this: any, ...args: any[]) {
        console.log("LOG: Entering method.")
        const result = originalMethod.call(this, ...args);
        console.log("LOG: Exiting method.")
        return result;
    }
    return replacementMethod;
}


class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    @loggedMethod
    greet() {
        console.log(`Hello, my name is ${this.name}.`);
    }
}
const p = new Person("Ray");
p.greet();
// Output:
//
//   LOG: Entering method.
//   Hello, my name is Ray.
//   LOG: Exiting method.
```

## Explicit Resource Management (5.2)

https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#using-declarations-and-explicit-resource-management

Typescript has added a "Disposable" interface to match a new JS proposal, as well as a `using` statement:

```js
function loggy(id: string): Disposable {
    console.log(`Creating ${id}`);
    return {
        [Symbol.dispose]() {
            console.log(`Disposing ${id}`);
        }
    }
}
function func() {
    using a = loggy("a");
    using b = loggy("b");
    {
        using c = loggy("c");
        using d = loggy("d");
    }
    using e = loggy("e");
    return;
    // Unreachable.
    // Never created, never disposed.
    using f = loggy("f");
}
func();
// Creating a
// Creating b
// Creating c
// Creating d
// Disposing d
// Disposing c
// Creating e
// Disposing e
// Disposing b
// Disposing a
```