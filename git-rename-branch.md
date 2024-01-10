---
title: Renaming Git Branches
date: 2020-08-17T23:00:13.630Z
tags:
  - git
  - programming
  - command-line
  - dotfiles
publish: true
canonical_url: https://zanca.dev/notes/git-rename-branch
---

## TL;DR;

```bash
# Checkout the branch you wanna rename
git checkout <old_branch_name>
# Rename the branch
git branch -m <new_branch_name>
```

## (Optional) Create a git alias
Either by running
```bash
git config --global alias.rename "branch -m --"
```

or by editing your `~/.gitconfig`

```toml
[alias]
  rename = branch -m --
```

Then you can just use:
```
git rename <new_branch_name>
```

> [!example]- Here's all the git aliases I use, if you were curious
> <iframe frameborder="0" scrolling="no" style="width:100%; height:311px;" allow="clipboard-write" src="https://emgithub.com/iframe.html?target=https%3A%2F%2Fgithub.com%2Fmetruzanca%2Fdotfiles%2Fblob%2Fmaster%2Fhome%2F.gitconfig%23L7-L19&style=github-dark&type=code&showCopy=on"></iframe>

