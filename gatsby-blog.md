---
title: Creating a Personal Website using Gatsby.js and Netlify CMS
slug: personal-portfolio-blog-gatsby-netlify-cms
pubDate: 2020-12-20T20:36:56.280Z
description: How I created my blog and portfolio website
tags: ["react", "netlify"]

---

One of my greatest weaknesses is perfectionism. This is most prevalent in my personal projects where I have no outside stimulus, such as a deadline, to keep me focussed on creating the most value for the least effort. In my personal projects I have a tendency to want to overthink (because it's fun!) the problem and try to come up with creative solutions.

Whiling making something, I often end up researching some topic that came up and soon I find myself deep on a random topic (like this week was cryptocurrency). While this does add to my personal knowledge base and in some cases experience (as I may start another experimental project to apply what I learned), I often find that I deviated far from the original goal I had set.

This blog and portfolio, will hopefully be a step in the right direction to stop that bad habit of mine. Instead I will try to actually finish my personal projects and take note of topics that I would like to research further and schedule a specific time to explore and research those ideas.

This specific blog post was written during the processes of creating this site, so this will serve as a step by step guide for anyone reading and more importantly my future self.

## Setting up Gatsby

After initially downloading the full example (thats using Netlify CMS and Gatsby) in order to get a feel for how they worked together, I decided to start from scratch following [the Netlify Docs](https://www.netlifycms.org/docs/gatsby/).

Following the guide on gatsby, I created a new gatsby site.

```
gatsby new blog https://github.com/gatsbyjs/gatsby-starter-blog
```

After this, I knew I wanted my site to be built in Typescript and styled with Styled-Components. To configure these two, I made use of Gatsby's Recipe feature. Running `gatsby recipes` we get a list of all available recipes. From that list I found the ones I was interested in (and looked up a few others for future use)

```
gatsby recipe typescript --install
```

To keep things neat, just in case, I actually decided to run the commands separately and make a commit for each of them.

> Don't forget to set your git identity before committing! <br>
> Coincidentally, I've got a post on that over [here](/blog/manage-git-identity).

```
gatsby recipe styled-components --install
```

## Setting up Netlify CMS

Add the netlify cms dependencies

```
npm install --save netlify-cms-app gatsby-plugin-netlify-cms
```

Add the config file (which is setup to work with how gatsby's template is by default)

```
├── static
│   ├── admin
│   │   ├── config.yml
```

```yaml
backend:
  name: git-gateway
  branch: master

media_folder: static/img
public_folder: /img

collections:
  - name: "blog"
    label: "Blog"
    folder: "content/blog"
    create: true
    slug: "index"
    media_folder: ""
    public_folder: ""
    path: "{{title}}/index"
    editor:
      preview: false
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Publish Date", name: "date", widget: "datetime" }
      - { label: "Description", name: "description", widget: "string" }
      - { label: "Body", name: "body", widget: "markdown" }
```

This configuration allows placing the post assets along side the markdown files. Which I actually prefer to how Netlify's example project was setup.

```
content/
├── blog
│   ├── first-post-title
│   │   ├── index.md
│   │   └── post-image.jpg
└── └── second-post-title
        ├── index.md
        └── post-image.jpg
```

The last step for Netlify CMS is to add the plugin to the `gatsby-config.js` file.

```js
plugins: [`gatsby-plugin-netlify-cms`]
```

With all that done, in theory Netlify CMS should be correctly configured and all that remains is to push to github and add the project to Netlify.

Once added Identity Service and Git Gateway needs to be enabled.

## The real coding begins!

Now that I've got everything working by following the guide from netlify's docs, I can get started on making my own changes.

### Getting /blog/ in my posts path

First and foremost, I need to get a /blog/ path before all my blog posts. If I don't it'll be messy when I add pages later and will absolutely screw up SEO ranking if I change tha page's URL. So the earliest I get this done the better.

To get this done I'm going to edit the `gatsby-node.js` file and within the `createPages` function where it makes a call to the `createPage` callback I need to edit the path.

```diff
  createPage({
+   path: `/blog${post.fields.slug}`,
    component: blogPost,
    context: {
      id: post.id,
      previousPostId,
      nextPostId,
    },
  })
```

> For code blocks that have both language and diffing highlighting (like github does), you can do that with prism.js. (Gatsby comes with it out of the box) Instead of setting the language to e.g. js, you can use diff-js

### Adding comments

Next up is adding support for comments. Theres this really nice project called [utteranc.es](https://utteranc.es/) which allows you to use GitHub issue comments for blog post comments. For anything but a developer blog, this would be inconvenient. However since this is indeed a developer blog, I feel like this is a great addition to the setup.

Not only does it make it easy to handle comments but it also makes contributing to articles that much easier. Say my dream to be come a well known developer like [Dan Abramov](https://overreacted.io/) comes true and I want to allow people to contribute to my blog posts by adding translations, I think utterances will help with the whole process. Keeping the conversation around a blog post all in one place.
