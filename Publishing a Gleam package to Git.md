---
timestamp: 2025-03-17T22:25:46
publish: false
tags:
  - til
  - gleam
---

Publishing a package is easy! But if you want to publish a Git-based package there's a few things you might want to add to make the experience, as close to the authentic Hex publish.

## Why should you avoid publishing to Hex?

Gleam is trying to grow in maturity and adoption, so you should avoid publishing packages that aren't fit for production. The most common reason you might want to make a package, is to make packages to easily reusable across your own projects. Instead of wondering if someone might want to use your library, or if something like it already exists, you should just ask in the community first. If your package is indeed something novel and cool that people want, you'll get great feedback. At which point, you can probably publish to Hex.

Another reason you might want to publish to Git instead of Hex, is if you're still working on your package. You should avoid publishing work-in-progress packages. Anything you wouldn't want to be run in a production environment (e.g. in space, since [javascript runs in space now](https://www.theverge.com/2022/8/18/23206110/james-webb-space-telescope-javascript-jwst-instrument-control)...), maybe hold off from publishing to Hex.

## How to Publish a package to Git

The title might be a bit misleading/clickbait, since all you need to do is push your code to git, assuming your project root, contains a gleam.toml file.


## Other possible questions
### Monorepo project with multiple packages.

Not supported at the moment.
