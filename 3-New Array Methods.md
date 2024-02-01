# New Array Methods

_[back](./README.md)_

## Array.prototype.flat() (ES2019)

Given nested arrays, flattens one or more levels of the nested arrays into a single array.

```js
> const arr = [1, [2, 3], [4, [5, 6]]];
> arr.flat() // or arr.flat(1)
[ 1, 2, 3, 4, [ 5, 6 ] ]

> arr.flat(2)
[ 1, 2, 3, 4, 5, 6 ]
```

## Array.prototype.flatMap() (ES2019)

Effectively, calls `array.map(...).flat(1)`. Supposedly offers a slight performance improvement.

```js
> ['abc', 'def', 'ghi'].flatMap(p => p.split(''))
[ 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i' ]
```

## Array.prototype.at() (ES2022)

Basically the same as indexing (`a[0]`) but supports negative relative indexing.

```js
> const a = [1, 2, 3, 4];
> a.at(2)
3
> a.at(-2)
3
```

## Array.prototype.toSorted() (ES2023)

Like `Array.prototype.sort()`, returns a sorted array, but unlike `sort()` does not modify the original array.

```js
> const a = [5, 7, 2, 4, 8];
> a.toSorted((l, r) => l-r)
[ 2, 4, 5, 7, 8 ]
> a
[ 5, 7, 2, 4, 8 ]
> a.sort((l, r) => r-l)
[ 8, 7, 5, 4, 2 ]
> a
[ 8, 7, 5, 4, 2 ]
```

## Array.prototype.toReversed() (ES2023)

Like `Array.prototype.reverse()`, returns a reversed array, but does not modify the original array.

```js
> const a = [1, 2, 3, 4, 5];
> a.toReversed()
[ 5, 4, 3, 2, 1 ]
> a
[ 1, 2, 3, 4, 5 ]
> a.reverse()
[ 5, 4, 3, 2, 1 ]
> a
[ 5, 4, 3, 2, 1 ]
```

## Array.prototype.toSpliced() (ES2023)

Similar to `Array.prototype.splice()`, but returns a new array with the additions/deletions instead of modifying the array in place and returning deletions.

```js
> const a = [1, 2, 3, 4, 5];
> a.toSpliced(2, 2, 'yoink!')
[ 1, 2, 'yoink!', 5 ]
> a
[ 1, 2, 3, 4, 5 ]
> a.splice(2, 2, 'yoink!')
[ 3, 4 ] // CAUTION: returns deleted elements, not the changed array
> a
[ 1, 2, 'yoink!', 5 ]
```

## Array.prototype.with() (ES2023)

```js
> const a = [1, 2, 3, 4];
> a.with(2, 'tres')
[ 1, 2, 'tres', 4 ]
> a
[ 1, 2, 3, 4 ]
> a.toSpliced(2, 1, 'tres')
[ 1, 2, 'tres', 4 ]
```