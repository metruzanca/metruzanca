---
title: How to manage your Git Identity for Work and for Personal Projects
slug: manage-git-identity
pubDate: 2020-12-20T18:48:35.562Z
description: How to easily manage your GIT Identity
tags:
  - git
  - linux
---

Simply put, its the name and email that gets used in the Author field of your commits that show up in `git log` and on the git server.

## Why do you need to manage your GIT Identities?

If you're like me and enjoy using [GitHub](https://github.com) for personal projects and some private git server (most often a self-hosted [GitLab](https://about.gitlab.com) for work, you often need to remember to set your GIT user and email.

_otherwise you could end up in a situation like this_

![committed with wrong identity](./git-fail.jpg)


> Notice my two different git identities? Your boss might find this a little unprofessional...

## The Solution

If your workplace lets you use your personal github profile, you're [the lucky one](https://youtu.be/dMwK7RSVi7g?t=60), You can just set your global identity and never think about it.

```
git config --global user.name "John Doe"
git config --global user.email "John.Doe@example.dev"
```

If however you have 2 or more identities you use on the daily, you've got 2 paths.

### Opt. 1: Setting Global Identity

You could still set your global to your most used identity and then override per repository.

This is probably the easiest of the two. But IMHO leaves too much up to chance for accidentally forgetting.

### Opt. 2: Setting per repository, every time

Simply put, don't set your global identity, manually set it every time you create or clone a repository. If you forget, github will simply remind you set it.

Now of course, writing out those two lines can be annoying. So to solve this very first world problem of repetitive typing I've come up with this simple bash script. (sorry windows users)

```bash
# $1 is identity name
identity() {
  case $1 in
    work | google)
      git config user.name "Work name"
      git config user.email "workName@Google.com"
      echo "You are now committing as workName@google.com"
      ;;
    personal)
      git config user.name "personal name"
      git config user.email "personalName@gmail.com"
      echo "You are now committing as personalName@gmail.com"
      ;;
  esac
}
```

If you're unfamiliar with writing bash/reading bash scripts, the important bits are

```bash
case1)  # Defines a case (similar to case "case1":)
        # code goes here
;;      # ends a case block (similar to break;)
```

When defining a case you can also add a pipe (`|`) for an OR case.

The code we're running in each case is simply setting our git config and echoing a completion message.

I personally placed this function in my `.bash_aliases`, but so long as its sourced at some point when you open your console/terminal you can call it like this:

```bash
identity work
# You are now committing as workName@google.com
identity personal
# You are now committing as personalName@gmail.com
identity google
# You are now committing as workName@google.com
```

> This is changing your identity in the repository you are currently working cd'd into.
