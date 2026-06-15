---
title: Squeal - A database viewer I built in Rust
description: TODO
date: 2026-06-15T10:51:45
tags:
  - terminal
  - rust
  - databases
  - ai-agents
canonical_url: https://zanca.dev/blog/project-squeal
publish: false
---

Today we're going to be talking about Squeal, a little weekend project I started to solve a problem I had.

Quick disclaimer: I make use of agentic coding in this project so if you're not a fan of that, you're free to click away from this article.

A little back story: I used to be a big VS Code user for many years and I loved the extensions because you could have everything in the same app. For example I would use a Database Viewer extension inside a VS Code. But recently I moved to the Zed editor, which I am starting to fall in love with, However one side effect is that the extension support is quite limited at the moment. And one of the limitations is that you cannot make custom views. So a database viewer extension is out of the question at the moment. However I've been building a bunch of Terminal apps as of late, Mostly in Node and Golang, And since I recently started to write almost exclusively in Rust, I decided it was time to try and make a ratatui-based app. This way I could have my Database viewer lives in my Zed terminal And I could get pretty close to the ideal workflow That I had from VS Code.

I've been writing Rust for a few weeks using a couple of different side projects as learning grounds. I think the most beneficial learning came from starting a codecrafter project, specifically building my own shell, as well as working my way through the Rustlings exercise book. So I feel pretty confident reading and writing most Intermediate rust. Of course there's things that I am still very scared to touch, like anything to do with complex macro internals, Lifetimes and pointers since I've come from mostly pointer-less languages like JavaScript. Luckily though, from what I saw, all of the frameworks like Ratatouille, Axiom, GPUI all abstract away most of these problems so I don't really have to touch them very much. I can just jump straight in and have fun.

## Ratatui

I hadn't tried Ratatui before this project but I've heard many good things from YouTubers, shitposters on X, and a good friend of mine also from X, Aster. I knew this was a great framework.

My goal for this project was to solve my problem and to not waste too much time on it. I took the approach of agentic coding rather than handwriting trad coding, as I've been doing for the past few weeks. So while developing this project I definitely learned less than from my CodeCrafters project but I had a lot of fun nonetheless as I got to focus on the product design. I already knew that AI was fairly good at writing Rust code, It's very good at writing go and so I figured Rust was a step up. Rust has a lot more usage than Go, There's plenty of rewrite-in-Rust projects on GitHub so AI has plenty of things to study from.

My side projects use SQLite for the database for convenience and for a very small footprint So the main database I wanted to support was SQLite. What I needed from squeal was to be able to view table structure and data, I kept running into issues where my production database was behind, usually because AI generated a migration with an out of order timestamp and so it never got run. To complicate matters I tend to deploy my projects on Railway so I don't have access to the SQLite file locally (its on a docker volume on the server). So I would like to have something that I can install on a server very quickly. A small Rust TUI application is a perfect fit.

## Agentic Coding

I have a lot of friends who don't like AI in general. Any time I share something cool with them, they shoot back with, "Was this AI generated?" "Oh now that I know that it's AI-generated, it doesn't matter how cool I thought it was. It's now bad". The common criticism being that anyone could have made this app. And that's definitely true but there's a lot of requirements for this app to be made without AI. First you need to know Rust then you need to know Linux and command line intuition And UX design. And this latter half is quite substantial. While building this app I had a lot of back and forth changing how to interact with the app and kept trying to find something that felt better to use. If a Joe Schmo made a prompt based on just looking at/using my project, the resulting app would probably not be the same. Could another seasoned developer make something like this? Yes absolutely. But it probably would have taken them at least a couple of days instead of a couple of hours.

Another big part of how I use AI is that I do still care about the architecture. Even if I will never write a line of code, I will still go in and try to refactor things (with AI). As I know that while the app is still in its early stages it's easier to change and I know that certain things Become a massive maintenance burden later on and so while they're still fresh in my head, I can change them now. AI is not infallible. If you put slop in, you're going to get slop out, So keeping quality up is important.

Something that I still remain a little skeptical about AI coding is AI-generated test suites. It's very easy to generate a bunch of tests and then whenever you change code, to just update all the tests that fail, regardless of if they're relevant or not. And a lot of developers do this too, either because of laziness or just because they don't know better. We looked at a lot of places that just didn't have test coverage or Bad testing policies meant we would just update the tests alongside the code and not really care about the tests. The standard once a metric becomes a metric, it's very easy to game the metric.

But I think it did an okay job this time around as when I went to refactor some of the internals of the database drivers, None of my tests ended up failing meaning that my refactors were atomic. But it's something I still remain somewhat skeptical about as it's very easy to overlook giant test files with a lot of changes.

## Squeal

Now to talk about Squeal! Squeal is a TUI application, Very much inspired by the likes of HTOP, nano, opencode, terminal.shop. Squeal uses the alternate screen approach, which means that whenever you open the application, it switches out the buffer of the terminal so that when you quit, it goes back to whatever you had on your terminal before, Instead of the approach of clearing the terminal Screen and drawing on it, Something you might do in a CLI.

The usage of Squeal is pretty simple by design. You just pass it the sqlite file or a Postgres connection string and it should just show you the tables and you should be able to view the contents. The controls should be intuitive and support many different ways of interacting with the app Such as arrow keys and VIM hjkl support. I wanted a very uncomplicated experience.
