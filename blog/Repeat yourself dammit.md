---
timestamp: 2024-04-05T19:25:49
publish: true
tags:
  - programming
  - best-practice
title: Repeat yourself, dammit!
description: Why you shouldn't use DRY; Optimize for change first.
date: 2024-04-05T19:39:21
canonical_url: https://zanca.dev/blog/Repeat-yourself-dammit
---

## Dry is Dead and we Killed it.

Stop using DRY or making everything into a component/function. DRY leads to complexity. Sure, there might be less of it, but theres a higher density of complexity. I'd rather have more code, that is simple.

One of my favorite scenarios: having like 3 different wrappers of 1 tool. What if instead we didn't write a wrapper and just used the tool raw? Thats way less code, and way less complex since we didn't have to constantly add features to the wrapper to re-expose features of the original tool. And if we need to know how to use the tool, you've got plenty of examples of that vs one really convoluted and generic-ified usage of it.

We should instead use WET(write everything twice/thrice) or AHA(avoid hasty abstractions). Another way to put it is:

> Optimize for change first.

Which is exactly what I described in the above example.

## What other people  say

Other people have said this before and likely did a better job.

- [AHA Programming](https://kentcdodds.com/blog/aha-programming) by Kent C. Dodds
- [The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) by Sandi Metz
- [Please do repeat yourself (DRY is dead)](https://dev.to/ralphcone/please-do-repeat-yourself-dry-is-dead-1jbg)
- [Do Repeat Yourself](https://tedspence.com/do-repeat-yourself-528230cb92f0)
- [Do Repeat yourself](https://accu.org/journals/overload/27/151/teodorescu_2662/)
