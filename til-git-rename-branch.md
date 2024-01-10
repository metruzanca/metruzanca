---
title: TIL Renaming branches
date: 2020-08-17T23:00:13.630Z
description: How to rename GIT Branches locally and remotely
tags: ["software-development", "git", "til"]
publish: true
canonical_url: https://zanca.dev/blog/til-git-rename-branch
---

_TL;DR below_

Today I wanted to move my `metruzanca/metruzanca.github.io` to a different repository for two reasons:

- First, so that I could host my portfolio without all my other gh-pages defaulting to `zbest.dev/<repo name>`
- I don't like master being the only folder allowed for gh-pages and instead having a dev repo in place of master.

So I decided to move clone my repo and then push it to a new repository `metruzanca/zbest.dev`.

After pulling i needed to rename a few things:

```bash
master -> gh-pages
dev -> master
```

Renaming a branch locally is easy enough

```bash
git checkout old_branch_name

git branch -m new_branch_name
```

I Initially thought that would be enough, but when i pushed to the new repo i noticed that when using commands like `git status` that the now gh-pages branch was still referencing origin/master (which was now a different branch aka what was origin/dev).

As you can imagine this, will create some havoc. So I realized that i needed update the upstream reference.

This is also easy enough

```bash
git push origin -u branch_name
```

For those curious, the -u flag does is the same as the --set-upstream flag. Which should look more familiar if you've ever run `git push` without specifying the origin name and remote branch. Git will be helpful and give you a copy-paste command to set the remote reference for that branch so that you can then use git push in the future while on that branch.

## TL;DR

```bash
# Checkout the branch you wanna rename
git checkout old_branch_name
# Rename the branch
git branch -m new_branch_name
# Update the upstream reference
git push origin -u branch_name
```
