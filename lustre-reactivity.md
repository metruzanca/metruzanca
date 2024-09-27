---
canonical_url: https://zanca.dev/blog/lustre-reactivity
title: Lustre's reactivity model
description: Taking a look at Lustre's reactivity model and comparing it with Reactjs and Solidjs approaches
tags:
  - gleam
  - lustre
  - frontend
timestamp: 2024-09-27T16:11:42
publish: false
---

> [!abstract]- Draft

- [ ] Look at how lustre updates the dom / elements / blows things away (react vs solid approach)
- [ ] Look at when lustre rerenders children.
  - [ ] How it handles rendering lists of items. Adding element to beginning and end of lists. (solid)
- [ ] Does lustre have "fine grained reactivity"
- [ ] Include lots of screenshots and code examples.
- [ ] How do you translate a big json data structure to a lustre model and what happens when you update a top level node, a mid node and a leaf. How do the children react? What causes them to rerender?

---
- [ ] Find the frontend performance benchmark tests that RyanCarnianto likes to use and build one for Gleam+Lustre. Maybe contribute to that repo.

## References
- https://dev.to/ryansolid/a-hands-on-introduction-to-fine-grained-reactivity-3ndf
