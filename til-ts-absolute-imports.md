---
title: TIL Absolute Imports with Typescript
date: 2020-08-16T23:39:44.486Z
description: How to setup Absolute Imports in a Typescript project
tags: ["typescript", "til"]
publish: true
canonical_url: https://zanca.dev/blog/til-ts-absolute-imports
---

If you hate seeing this:

```ts
// SomeContainer.tsx
import {MyComponent} from '../../components/MyComponent.tsx'
```

and would prefer to see this instead:

```ts
// SomeContainer.tsx
import {MyComponent} from 'components/MyComponent.tsx'
```

Heres the snippet you need:

```jsonc
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./src",
  }
}
```

## BONUS: Wanna import from just `components/`?

e.g.

```ts
// SomeContainer.tsx
import {MyComponent} from 'components'
```

Just add an index.ts inside the components folder (and any other folder you store types of components in)

```ts
// components/index.ts
// if using export default
export {default as MyComponent} from './Mycomponent'
// If using named exports
export * from './Mycomponent'
```

_Much cleaner, ain't it?_
