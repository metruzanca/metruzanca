---
title: Github Actions for automatic Github Releases
date: 2021-04-21T00:21:18.749Z
description: How to Automate Github Releases with a Github Actions pipeline for Continuous Deployment
tags:
  - til
  - devops
  - github
publish: true
canonical_url: https://zanca.dev/blog/gh-automatic-releases
---

I've really fallen in love with github actions. They make allow you to play around with continuos integration and enable so much automation for your personal projects.

If you've ever wanted to make automatic releases on github, look no further.

## Step 1: Create the workflow file

Create a new file at this path

```bash
.github/workflows/release.yml
```

If you're using vscode you can simply click new file and paste that path in. If you're going bare with a terminal have this:

```bash
mkdir -p .github/workflows && touch .github/workflows/release.yml
```

## Step 2: The Workflow itself

But first TL;DR: We're using [Zip Release](https://github.com/marketplace/actions/zip-release)

And heres the code:

```yaml
name: Release
on:
  push:
    tags:
      - '*'
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Create Archive Release
        uses: thedoctor0/zip-release@master
        with:
          filename: 'release.zip'
          exclusions: '*.git*'
      - name: Upload Release
        uses: ncipollo/release-action@v1
        with:
          artifacts: 'release.zip'
          token: ${{ secrets.GITHUB_TOKEN }}
```

The long version. This action will only trigger when we push tags to the repository. Once triggered, it will first checkout the master(or main) branch using the "native" action for checkout. Afterwards it will zip using thedoctor0's [Zip Release](https://github.com/marketplace/actions/zip-release) to zip up the contents of the repository and use the [Release Action](https://github.com/ncipollo/release-action) to create a new release using the newly created zip file as artifact.

You could conceivably create multiple zips or other types of packages (e.g. jars) to use as artefacts.

An example of artefacts is the releases of my personal minecraft [texture pack](https://github.com/metruzanca/minecraft-xpd-texture-pack

) where the pack is distributed as a .zip file.

> In case you're also wondering what `secrets.GITHUB_TOKEN` is, I've asked myself that too. Basically its a token github can use within the action to authenticate as "the repository" in order to perform actions within the repository e.g. creating new releases. You can read more over at the [Docs](https://docs.github.com/en/enterprise-server@3.0/actions/reference/authentication-in-a-workflow).

## Step 3: Creating new releases

With the workflow created, unlike in my other guide for a [gh-pages deploy pipeline](https://zanca.dev/blog/continuos-deployment-of-a-static-site-to-github-pages/) which was triggered by pushing to master, meanwhile this workflow is triggered by pushing tags.

> Whats a tag? Its basically a bookmark to a certain commit. Allowing you to easily say checkout tags/v1 or tags/v1.1

TL;DR

```bash
git tag -a v<semver> -m "<release message>" # Creates a tag
git push --tags # This triggers the release
```

Now, theres a good chance that you'd never touched tags. So heres a quick guide:

To view your tags (q to exit as per usual)

```bash
git tag
```

Creating Annotated tags:

> Tags come in two flavours, normal tags and annotated tags. Annotated tags are best practice as they contain a lot of information (such as tag author), however you can use normal tags as temporary tags.

```bash
git tag -a v<semver> -m "<release message>"
```

This is akin to making a commit, it stays local until explicitly pushing to remote. To do that we simply need to push the tags. `git push` will push everything but if you only want to publish tags theres a flag for that.

```bash
git push --tags
```

The --tags flag is also available on fetch

```bash
git fetch --tags
```

## Step 4: Profit

If everything is setup properly, pushing tags should trigger a github action and create your release package.
