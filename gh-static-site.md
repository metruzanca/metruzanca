---
title: Deploy website to GitHub Pages
date: 2020-08-17T23:35:00.530Z
description: How to easily setup a CI pipeline with github actions to deploy a Static Site (e.g. React or Gatsby) to Github Pages
tags:
  - git
  - devops
  - til
publish: true
---

I often create **React** or **Gatsby** apps and usually just opt to use github pages as a host.

Up until now my process for getting the `build/` directory to where it needs to be, has been... messy to say the least. Today I've gone ahead and automated that once and for all and I've found what I think is a nice and easy solution. One thats also easy to setup for new repository.

Here's how it works.

## Step 1: Create the workflow

Go ahead and create a new file at `.github/workflows/deploy.yml` with the following contents:

```yaml
name: Deployment
on:
  push:
    branches:
      - master
jobs:
  deploy:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]
    steps:
      - uses: actions/checkout@v1
      - name: Use node ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - name: Install dependencies
        run: npm ci
      - name: Build site
        run: npm run build
      - name: Deploy to gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./build
```

The gist of this workflow is two fold:

1. A first part which simply installs dependencies and runs the build script.
2. A second part which uses a nice third party action [Actions GH-Pages](https://github.com/peaceiris/actions-gh-pages) workflow which makes deploying to a gh-pages branch really convenient.

The first part works out of the box (assuming you've used either create-react-app or gatsby-cli to bootstrap a project and thus have a npm build script), to get the second part working we need to setup a token for deploying. We've got a couple of [options](https://github.com/peaceiris/actions-gh-pages#supported-tokens) however I think the best and most convenient one is using ssh deploy keys.

## Step 2: Create a new ssh key pair

Very important that you create a **new ssh key pair** and don't use one you might already be using if say you're using ssh with github. The reason behind this is that we're creating a key pair that our workflow will use.

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

## Step 3: Add the ssh keys to the repository

After generating the keys go over to your repository settings and add the following.

- In **Deploy Keys**, add your public key with the Allow write access.
- In **Secrets**,  add your private key as ACTIONS_DEPLOY_KEY. (same name used in the workflow)

## And We're Done!

The next time you push to master, this workflow will be triggered and will install, build and then create a new branch for us.

_The last optional step would be to play around with the settings for gh-pages e.g. Adding a custom domain / subdomain._
