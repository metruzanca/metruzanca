---
title: Barrel Files - pros and cons
timestamp: 2025-02-28T17:14:59
tags:
  - typescript
  - javascript
publish: false
description: "A breakdown of the pros and cons to Barrel files; Spoiler: it's mostly bad!"
canonical_url: https://zanca.dev/blog/javascript-barrel-files
---
## Pros
- Its pretty.
- It's nice for libraries.

## Cons
- Barrels also make it impossible to import some code without accidentally running other code.
- I used to love barrel files, but now I just hate them. They make finding if something is used or not more difficult.
- it makes optimizers a pain since you have to not break all the sideeffects created from the export chain in case they're needed for something, it's a big problem in the Bun bake dev server
    - Debugging side-effects of not importing a bunch of files, can be difficult. (ideally you wouldn't have side effects like this but often it happens anyways)



---

You should stop caring about your imports. This is made easier