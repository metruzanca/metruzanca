---
draft: true
title: "Minimalist Git"
slug: "minimalist-git"
pubDate: 2000-01-01T00:00:00.000Z
description: The only subset of git commands that you need
tags: ["git", "programming"]
heroImage: '/minimalist-git.jpg'
---

After many years of getting myself into sticky situations with git, I've that some methods just work better.
I'm not going to be explaining the basics of git, I'll just explaining how I like to use git and how that's saved me hassle.
The TL;DR; of this article, is to just keep your history clean. If you don't know how to do that, don't worry, I didn't for a long time.

## Keeping your Git History Clean
The main culprit of a messy git history is the **Merge** command and to make matters worse, we tend to call many things "merging":
- Rebasing
- Fast-Forwarding
- Actual Merging

> Honorable mention to "squash" "merge"

Read up on [merge & --no-ff](https://www.perplexity.ai/search/9caf6d47-e38b-4332-ae92-59fc06c62e95?s=c) and write more here

rebasing

## Dropping & Cherry-picking

when you need work other devs did on another branch, which may or may not been made when it was ahead or behind of master

## When force pushing is acceptable
in feature branches

commit --amend

## Other tips
I found that I like making "DRAFT" commits and "undoing" them rather than using stashes, most of the time.