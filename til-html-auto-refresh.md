---
title: How to Auto Refresh an HTML page without Javascript
slug: how-to-auto-refresh-an-html-page-without-javascript
pubDate: 2020-09-30T23:37:22.091Z
description: How to make a website auto refresh without Javascript
tags: ["til", "web-development"]
---

Apparently theres a meta tag that auto refreshes your page:
```html
<meta http-equiv="refresh" content="30" >
```
In this case this will auto refresh the page every 30 seconds. Useful for if you're designing a very simple dashboard thats meant to be on display data but isn't going to be interacted with very often or can't e.g. a TV on the wall of an office. Great if you don't want/need to implement complicated logic to update the data.
