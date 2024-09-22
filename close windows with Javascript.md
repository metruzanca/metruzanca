---
title: How to close the current window with JavaScript
description: Explaining the quirks and workaround for closing windows in Javascript
tags:
  - javascript
  - til
timestamp: 2024-09-22T16:03:18
publish: true
---

> [!example] tldr
> ```js
> window.open(location, '_self').close();
> // or
> window.close('', '_parent', '');
> ```
> Working as of Chromium: `128.0.6613.138`

Today I learned a little quirk with closing the current tab/window with Javascript.

So apparently, you cannot use `window.close();` to close the current window, unless that window was programmatically opened. But you can close children windows if you kept a reference to them. As it turns out, the children can also close the parent window. And for some reason, if you call `window.close('', '_parent', '')`, this actually works.... but it shouldn't.

`close` doesn't actually take any arguments... (_also see [mdn](https://developer.mozilla.org/en-US/docs/Web/API/Window/close)_)

![[js-window-close-args.png]]

Reading the [html spec](https://html.spec.whatwg.org/multipage/nav-history-apis.html#dom-window-close-dev), `close` doesn't have args either.

Searching the internet, the only thing I could find was this [post](https://natclark.com/tutorials/javascript-close-tab/) by natclark.

Apparently `_parent`, much like the widely known `_blank` has to do with `a` tag targets. There's actually more targets i.e.: `_self`, and  `_top`.

Here's an interesting graphic explaining the targets from [stackoverflow](https://stackoverflow.com/questions/18470097/difference-between-self-top-and-parent-in-the-anchor-tag-target-attribute).

![[js-browser-url-targets.png]]

My best guess, is that this works purely for backwards compatibility. There's even a [post](https://lonare.medium.com/window-close-not-working-how-to-redirect-the-parent-window-i-have-got-you-covered-solution-56c63a40f169) talking about a similar workaround/exploit where you can call: `window.open(location, '_self').close()` that seems like it worked the same way.

I believe, that close takes arguments for backwards compatibility purposes, but the arguments are deprecated, so they're not documented anymore. So if browsers lost this ability, you couldn't blame them since its not in the docs.

`window.close('', '_parent', '')` seems to work because close tries to get the parent of the current window, finds that it is the top parent, so it then closes it's parent aka itself. Honestly, kind of strange still, the other workaround makes more sense.

`window.open(location, '_self').close()`: on the other hand, tries to open a window, but since `_self` already exists, then it gets reused and then we can call close on it, but since it was "opened" via Javascript, it no longer is blocked.

> ##### 7.2.2.1 Opening and closing windows
>  [...] If a window already exists with that name, it is reused.

Both workarounds seem to work in Chromium: `128.0.6613.138` (Official Build) (64-bit).
