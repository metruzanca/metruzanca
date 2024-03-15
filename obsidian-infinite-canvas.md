---
title: Exploring Obsidian's Infinite Canvas format jsoncanvas.org
description: The Obsidian.md team released an open file format called JSON canvas. Let's deep dive and play around with it!
date: 2024-03-14T09:14:24
tags:
  - open-source
  - frontend
  - obsidian
canonical_url: https://zanca.dev/blog/obsidian-infinite-canvas
publish: false
---

## [DRAFT] notes

Things I want to explore/do:

- Pick apart their site to find:
  - out how they handle things like panning around
  - Loading files
  - Draw arrows
- Create a version of their site that allows previewing jsoncanvas data.
- Create a library version of json canvas for use in react/solid/svelte (mainly solid)

(https://www.npmjs.com/search?q=jsoncanvas isn't taken yet!)

Well fuck!

![[Pasted image 20240315110236.png]]

---
<!-- INTRO -->

I've always found infinite canvas sites/apps spectacles. As a developer with a keen interest in UX, infinite canvas apps always felt so sleek and gave off vibes of only "real developers" could make this, as we all know frontend isn't real engineering (sarcasm).

Last year I almost started working at [quadratic](https://quadratichq.com), which have their own infinite-canvas app, except I bombed one of their technical interviews by not having a cool enough side project to show off. (_All my coolest work has been at work thus far_). After embarrassing myself, I tried to make my own infinite canvas implementation using the html canvas approach. Here's a [live demo](https://zanca-infinite-canvas.vercel.app/) and the [repo](https://github.com/metruzanca/infinite-canvas) (_warning: it's a bit spaghet_). Making that was very educational and overall interesting and fun to work on (_and has since been pinned to my GitHub profile_). Raw-dogging the perspective with canvas is a lot of work, if I were to make a serious app using the html canvas approach, I would probably use [pixi.js](https://npmjs.com/package/pixi.js) in conjunction with [pixi-viewport](https://npmjs.com/package/pixi-viewport), which is what quadratic uses, based on their [source code](https://github.com/quadratichq/quadratic/blob/69ecdfb5f5212dcfbf26e21a5e8fbb93c0d07726/quadratic-client/package.json#L52).

With Obsidian releasing their infinite canvas implementation...

## JsonCanvas site breakdown

[JsonCanvas.org](https://jsoncanvas.org/) does a few things that I think most developers, who like the frontend, will find interesting.

### Panning around

The biggest one in my opinion is how they manage panning around. (<kbd>space</kbd> + click drag)

When it comes to infinite canvas's theres generally two approaches:

- html canvas rendering ([figma](https://figma.com), [quadratic](https://quadratichq.com/), [excalidraw](https://excalidraw.com/) use this)
- css trickery ([tldraw](https://tldraw.com/), [d4builds](https://d4builds.gg/build-planner/)(skill tree page) use this)
### Rendering Edges

### Rendering Nodes

## Reverse Engineering

When I first saw the site, I knew I wanted to reverse engineer it and make my own version. Specifically, I wanted to make some kind of jsoncanvas editor & components. Initially, I thought it was all contained in the [canvas.js](https://github.com/obsidianmd/jsoncanvas/blob/bfd0553285678503b8112bf47b4f0eb1bbcc2bdc/assets/canvas.js) file, but actually theres some important things stored inside of [`/.layouts/default.html`](https://github.com/obsidianmd/jsoncanvas/blob/bfd0553285678503b8112bf47b4f0eb1bbcc2bdc/.layouts/default.html) as well.

The first thing I'd like to do is move all the canvas related code to a self-contained component.

<!-- TODO make framework agnostic components -->

### Tips

In vscode you can add the following for syntax highlighting.

```json
"files.associations": {
  "*.canvas": "json"
}
```
