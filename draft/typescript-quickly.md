---
title: Learn Typescript Quickly
date: 2023-09-08T11:22:47.574Z
description: Learn the Basics of Typescript in 5 minutes
tags:
  - typescript
  - react
publish: false
---

## Learn Typescript in 5minutes

The best way to learn typescript is to start using it. Take a `.js` file and rename it to `.ts`. (if it has react JSX in it, name it `.tsx` instead.)

From there, just fix the red _squigglies_.

> "But how?"

Step 1. Read this page: [Ts basics](https://www.typescriptlang.org/docs/handbook/2/basic-types.html). This is a great primer. Don't try to study this page. Just read thru to get an _idea_ and come back to it later.

Next open a project, and start changing `.js` files to `.ts`. Use the docs or ask [Perplexity](https://perplexity.ai) to explain it to you. If you provide perplexity with a snippet and the error message + a question, it does a reasonably good job at telling you whats wrong and it might even link your some more resources.

### Best Practice Examples - js vs ts

Below are some condensed snippets to help you move your code from js to ts.

### Variables
```js
let value = 1
let arr = []
arr.push(value)
```
```ts
let value = 1
// ts can't figure out that you want an array of numbers, so we have to be explicit here
let arr: number[] = []
arr.push(value)
```

Generally speaking, you should avoid adding types when type inference has got your back.

Doing things like this is redundant and gets old fast.

```ts
let num: number = 1
```

### Functions
```js
// js
function myFunc(value) {
  return value + 2
}
const myArrow = value => value + 2
```

```ts
// ts
function myFunc(value: number): number {
  return value + 2
}
const myArrow = (value: number): number => value + 2
//                                ^? this type, is the return of the function.
//                    it is optional, since ts can "figure out" the return type based on what you return
//                    this is whats called "type inference"
```

### Generics basics

In typescript you'll often see `<>` being used after types/functions. These are called Generics. They're essentially a way to pass types, instead of values.

I would avoid making custom generics until you're more familiar with the basics, but heres what you do need to know.

> The best way to show you how generics work, is to show you some code of how you would make use one.
> You won't have to do this part yourself, but this should help explain the next snippet.

```ts
function identity<T>(value: T) {
  return value
}

// or as an arrow function
const identity = <T>(value: T) => value 
```

In both cases we've got an `identity` function that returns the value you pass in. This function can be used just like any other function.

```ts
identity('Hello there.')
```

If you plop this into an editor, you won't get any errors. You can pass in any type you want and you won't get errors.

Now lets try using the generic

```ts
identity<string>(1)
```

This on the other hand will give you an error.

```md
Argument of type `number` is not assignable to parameter of type `string`.
```

We've effectively constrained what kind of value we can pass into the identity function.

> "How is this useful?" Stay tuned.

### Objects

```js
const obj = {}
obj.name = 'sam'
```

With objects, this is where you start to change how you design your code.

In javascript we can easily just not specify that obj _will_ have name (but we then won't get suggested that name might be there).

In typescript, we have to state all the keys up front and provide them with default values.

In the long run this is a better practice, because this cuts out a lot of `if (obj.name) /* do something with obj */` type of code (_usually called "defensive programming"_).

```ts
const obj: { name: string } = { name: '' }
obj.name = 'sam'
```

This can be cleaned up a bit by moving the type to a `type`.

```ts
type MyObject = { name: string }
const obj: MyObject = { name: '' }
obj.name = 'sam'
```

`type`s just like functions/variables can be exported and imported.

> Note, typescript have `type`s and `interface`s. They're pretty similar but interfaces have some extra, non-intuitive features, making them more error prone. I would recommend avoiding them for the most part.

<details>
<summary>Escape Hatch</summary>

There are of course scenarios where we maybe don't have the data yet, but will have it later. I would urge you to try to try and adapt your design.

You'll usually end up with simpler code. But if you can't figure it out, heres an escape hatch.

```ts
type Data = { name: string }
const getData = (): Data => {
  const data: Partial<Data> = {}
  // ..
  data.name = 'sam'

  return obj as Data
}
```

> Escape hatches, shouldn't be abused as you're not allowing typescript to do its job and you might end up with more bugs. With escape hatches, you're effectively saying "trust me bro, I know what I'm doing".

Partial is a generic type that modifies all the keys and makes them optional.

We're then able to the keys at our convenience and then before we return we use `as Data` which tells typescript to treat this object as if it we of type Data.

> Its very important to note that if name was never set, for example, and you try to use it, you won't get typescript errors but javascript errors. Ideally we want to keep all errors in typescript as we'll know about them when we write the code, not when our customers call us angrily.

</details>

### React Component prop types

If you're using propTypes, you can throw those away. Typescript does what that library does and better.

> If you're not using the `prop-types` package, skip down to the typescript snippet

```js
// js
import React from 'react
import PropTypes from 'prop-types';

const MyComponent = ({ name }) => {
  return <div>Hello, {name}!</div>
}

MyComponent.propTypes = {
  name: PropTypes.string.isRequired
}
```
<!-- TODO [twoslash + highlighter](https://fatihkalifa.com/blog/typescript-twoslash) -->
```ts
// ts
import React, FC from 'react
type Props = {
  name: string
}
const MyComponent: FC<Props> = ({ name }) => {
  return <div>Hello, {name}!</div>
}
```
> FC is a type just like Props, but FC accepts a generic much like identity from earlier

In both these snippets if name isn't provided, you'll get a compile error. This is because name is required.

If instead you wanted it to be optional you would do this:

```ts
// ts
import React, FC from 'react
type Props = {
  // question mark denotes that name is optional.
  // Its equivalent to `name: string|undefined`
  name?: string
}
const MyComponent: FC<Props> = ({ name }) => {
  return <div>Hello, {name ?? 'World'}!</div>
}
```

In react, its best practice to use Arrow functions for components, since "traditional" functions don't have access to FC.

This is the equivalent in using traditional functions:

```ts
// ts
function MyComponent(props: Props): JSX.Element {

}
``` 

---
### Heres some Resources:
- the [official docs](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
- [google](https://google.com)
- AI tools like [Perplexity](https://perplexity.ai) (my favorite way as of recently).
- [React's learn typescript](https://react.dev/learn/typescript)
- Heres a neat [recap](https://basarat.gitbook.io/typescript/recap) on what typescript does for us as developers.
