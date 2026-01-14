---
timestamp: 2025-09-02T16:15:33
publish: true
tags:
  - gleam
  - functional-programming
aliases:
  - TCO
title: Tail-Call Optimization (in gleam)
description: Tail-Call Optimization is crucial for recursive algorithms, here's how it works.
canonical_url: https://zanca.dev/blog/tail-call-optimization
---

Tail-Call Optimization is compiler optimization that allows a recursive function, where the last line of the function is the call to itself, to reuse the same memory stack, thus allowing the recursive function to use constant memory.

> ![[hayleigh-on-tco.png|350]]
>
> proof: `Specification and Transformation of Programs by Helmut A. Partsch; Chapter 6`

## Why does this matter?

Tail-Call Optimization is crucial for recursive algorithms, especially those that might involve deep recursion. It prevents "stack overflow" errors by reusing the call stack for tail calls, ensuring constant memory usage. This makes highly recursive functional code robust and performant, avoiding crashes and allowing elegant solutions that might otherwise require iterative rewrites.

### Feels Like Mutation (But Isn't)

In functional programming, mutation is typically avoided. However, by using accumulators in tail-recursive functions it can feel like to mutable state. The accumulator parameters effectively "store" the evolving state of the computation. With each recursive call, new values for these accumulators are passed, creating a new "snapshot" of the state rather than directly altering an existing variable. With TCO, we maintain immutability and get simpler code as a side effect.

The only negative in Gleam is you'll have to make a public facing wrapper function to hide the accumulator parameters since Gleam doesn't support optional or default arguments.

## Example

Every recursive function can be turned into a tail-recursive one. So lets start with a classic example of Fibonacci, since its often used to explain recursion and we usually see it in its non tail-recursive form.

```rust
fn fibonacci(n: Int) -> Int {
  case n {
    0 -> 0
    1 -> 1
    _ -> fibonacci(n - 1) + fibonacci(n - 2)
  }
}
```

To make it tail-recursive, we need to make sure the last call is a single call, not two. If you think about how a human would solve this, they would go in pairs of two: 0 + 1 is 1. 1 + 1, is 2, 1 + 2 is 3, 2 + 3 is 5. So lets add 2 accumulator values to keep track of our pairs.

```rust
// Private, actual tail recursive function
fn fibonacci_tail(n: Int, a: Int, b: Int) -> Int {
  case n {
    0 -> a
    1 -> b
    _ -> fibonacci_tail(n - 1, b, a + b)
  }
}

// Public interface that simplifies the parameters
pub fn fibonacci(n: Int) -> Int {
  fibonacci_tail(n, 0, 1)
}
```

```rust
// Both function the same
pub fn main() {
  10
  |> fibonacci
  |> echo // 55
}
```

Here's all the `fibonacci_tail` calls. As you can see `a` and `b` are basically holding all the fibonacci digits and slowly stepping thru the sequence.

```rust
fibonacci_tail(10, 0, 1)
fibonacci_tail(9, 1, 1)
fibonacci_tail(8, 1, 2)
fibonacci_tail(7, 2, 3)
fibonacci_tail(6, 3, 5)
fibonacci_tail(5, 5, 8)
fibonacci_tail(4, 8, 13)
fibonacci_tail(3, 13, 21)
fibonacci_tail(2, 21, 34)
fibonacci_tail(1, 34, 55)
55
```

And here's a javascript version using an iterative approach.

```js
function fibonacciIterative(n) {
  if (n === 0 || n === 1) return n;

  let a = 0, b = 1;
  for (let i = 2; i <= n; i++) {
    let temp = a + b;
    a = b;
    b = temp;
  }

  return b;
}
```

_I don't know about you, but the gleam version is a lot easier to read and understand._

### TODO add more examples

---

### Compiler Support for TCO

TCO isn't universally supported. Languages like Gleam, OCaml, and Haskell offer TCO guarantees.

Conversely, many languages, including JavaScript, don't optimize tail calls by default. This can lead to stack overflows for deep recursion, unlike in languages with TCO. Always confirm your language and compiler's TCO support.
