---
title: 
description: 
date: 2024-01-10T04:15:22
tags: 
canonical_url: https://zanca.dev/blog/
publish: false
---
Kenny said his mentor told him that you never need to rebase. I disagree.

This reminds me of this video

![You only Git Merge?!? feat Theo](https://youtu.be/7gEbHsHXdn0)

IMO:
When working in a branch, and master gets an update, always rebase your branch to master. Never merge master into your branch to get those changes.
When your branch is ready to merge, you merge with squash. Never not squash and only merge

The problem with rebase, is that you have to deal with conflicts at every commit, instead of once at the end.
I an ideal scenario aka working on a team where everyone has deadlines and branches don't stay up for very long, this problem doesn't exist.

The problem with merge is it gives you a messy git history.



#TODO visualizations in excalidraw. e.g. something like these https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges

I like rebase because I like having a clean history, because I often find myself reading thru it for one reason or another.

It is possible to have a clean history with merging, so long as you squash. This way, if everyone squashes, you can merge and I can rebase. I find rebase also helps me keep a clean feature branch history, which makes reviewing PRs easier.

But overall, long running feature branches are the devil and you will run into all the problems.