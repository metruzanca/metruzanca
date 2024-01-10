---
title: You're likely using Obsidian wrong
description: Many plugins like Dataview or Templater, introduce unnecessary complexity and lock you into Obsidian, going against Obsidian's core idea.
date: 2024-01-09T15:06:59
tags:
  - obsidian
canonical_url: https://zanca.dev/blog/obsidian
publish: false
---

## [DRAFT]
### Simplicity and Universality

Obsidian is an exciting note-taking app that offers a fantastic user experience. As a developer, its knack for reusing existing notes aligns perfectly with how we segment code into reusable snippets. Obsidian is a tool that empowers non-coders with the same ability to break down and reuse knowledge, just like we do with code. Tools like Obsidian resonate well because they treat knowledge just like code - meant to be broken down and reused.

Obsidian leverages Markdown and it really leans into the philosophies behind markdown. **Simplicity** and **Universality\***.

Markdown is simple to use, no fumbling with toolbars or app specific formatting. Knowledge in one markdown editor is transferable to other apps that support markdown. Using markdown is generally faster, since its keyboard centric. Typing `##` for a heading instead of having to move your hand to your mouse and find the button to click. The more you practice with movement on the keyboard, you can really fly over your document. Files can also be easily restyled at a later point. You can see this any time you install a new theme to obsidian.

\*Obsidian's Markdown actually isn't universal, since many Markdown implementations don't support "wiki-links" aka "`[[link|optional-text]]`". However, obsidian is making them more popular, so that might change in the future. However, there's already tons of plugins for other tools to support wiki-links or convert them to traditional markdown links. However, that being said, swapping from Obsidian to something like: [Roam](https://roamresearch.com/), [Notion](https://www.notion.so/) or [Joplin](https://joplinapp.org/).

### Where Obsidian goes wrong

As much as I like plugins like Dataview and Templater, they break both philosophies of Obsidian (and thus markdown).

#### Dataview

%%
Still not sure where this blog post is going.

But I think it has to do with how dataview makes you make views for stuff instead of using text search, tags or back links. Its an alternative but is making you the user look for stuff in a long list instead of typing something out and getting the answer quickly.

Found a [reddit post](https://www.reddit.com/r/ObsidianMD/comments/15hb6tk/are_there_many_obsidian_users_who_do_not_use_the/) with some interesting points e.g.
- get most recent notes / recently edited. Since unlike folders on a file system, you can't sort by that in obsidian.

The intro of this video does a good job of hitting the points I want to cover with Dataview

https://www.youtube.com/watch?v=baKCC2uTbRc

%%

Dataview, as compelling as it is, it introduces a layer of complexity. Viewing your files as a database, is an interesting idea and while I'm not against using frontmatter (more on this later). But its really just a distraction that pulls you away from building knowledge.

#### Templater

 Templater has some good but also some bad. Generally, writing js in obsidian is where the bad starts. Templater being a better core-templates plugin is good. The folder templates are good. Getting wild with them is where it might lead to an issue

### The Frontmatter

 #TODO should mention how frontmatter is widely adopted. e.g. github will display the data in a table similarly to obsidian.
