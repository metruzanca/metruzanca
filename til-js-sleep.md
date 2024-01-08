---
title: TIL Sleep function in Javascript
date: 2020-10-17T23:50:38.008Z
description: How to make a actual sleep function in Javascript
tags: ["til", "javascript"]
publish: true
---

Occasionally if you'd like to put execution in pause, in other languages, this is done with a function called `sleep()` (or `wait()`). In javascript, you might think that this can be done with `setTimeout()`, but you'd be wrong. This will only queue a task to be executed later while the main script still continues executing to completion.

It is possible to promisify the setTimeout in a way to make it act like a sleep function but that solution isn't perfect.

Natively in js  however, theres a really cool thing called `atomics.wait()` which can be used to implement a sleep function.

`atomics.wait()` by itself does not cause the script to pause execution, but instead this function is checking that the shared-array we pass it contains a value and if so waits for a specified duration.

To be a bit more precise, heres an excerpt from mdn:

> The static Atomics.wait() method verifies that a given position in an Int32Array still contains a given value and if so sleeps, awaiting a wakeup or a timeout. It returns a string which is either "ok", "not-equal", or "timed-out". <br>
> *source: [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics/wait)*

```ts
function sleep(ms:number){
  Atomics.wait(new Int32Array(new SharedArrayBuffer(4)),0 ,0, ms)
}
```
> NB: if using js, remove `:number`, thats just a typescript type notation.
