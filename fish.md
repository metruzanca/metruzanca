---
title: How I use Fish Shell
description: Discover why the Fish shell offers an unparalleled out-of-box experience. Learn about its superior completions, navigation, and configuration, making it the best modern shell for developers.
date: 2025-08-29T12:06:51
tags:
  - terminal
  - linux
  - fish
canonical_url: https://zanca.dev/blog/fish
publish: true
---

Tired of endlessly tweaking your shell configuration? Fish offers a refreshingly opinionated and powerful experience right out of the box.

I love the Fish shell for several reasons, the first being the **out-of-box experience is best in class**. You don't need to go crazy with plugins or hell even a status line tool like `oh-my-zsh`. You can just get to work. Fish comes with so many nice features that make moving around in the terminal so pleasant. 

> [!example]- To name a few
> - Superior shell completions: not only do you get inline documentation, pulled straight for the man page, but the tab completion and options picking experience is just implemented better than all other shells.
> - Navigation shortcuts: `alt/option/super+left_arrow/right_arrow` will move between your cd history, so going back and forwards is very quick.
> - Command history: Fish proactively suggests commands based on history and completions as you type. Pressing `up` will of course like all shells cycle your commands, but if you type a command e.g. `cd` and then press `up` you'll filter to all usages of `cd` only. And this also includes if you've used cd at the end of your command e.g. if you've piped into it.
>    - Additionally, pressing `super`+`up` will cycle thru the parameters of your previous commands and you can do this at any point, so if you have a path in a previous command, you don't have to copy paste that path, you can just cycle to it with this shortcut.
> - Abbreviations: like aliases, but unlike them, they expand after you use them. This way your shell history will always be readable. You will never ask yourself: "I deleted cmd, what did that alias do again?"
> - Syntax Highlighting: Fish has syntax highlighting by default. It's a small but significant UX improvement.
> - Oh and its just faster. Even before the RustBTW rewrite.
> - And many more that I can't remember but use every day due to muscle memory.
> > Caveat: Nushell is very compelling too as it has very comparable UX to Fish, but fish is a more familiar shell. Nushell completely changes the paradigm and offers features that might not interest you. Worth noting that both Nu and Fish have their own shell language.
 

Another reason is **configuration**. Yes, Fish has its own scripting language which makes it not POSIX compliant. Most make this out to be a bigger deal than it actually is. People complain about it being a poor choice for scripting and issues with portability. The solution to both of these, is to use fish-lang for only its designed purpose.

System-level scripts should be written in sh/bash for portability. Your shell config is _personal_.

As for integrating with tools most tools out there have fish integrations, so using fish is almost never actually an issue. Tools like python virtual environments create `activate` scripts in fish if that's the shell they detect you using. And if you're using a tool written entirely in bash you have two options:

- Find an alternative written for fish or in a _real programming language_ and shell agnostic.
    - e.g. for node's [nvm](https://github.com/nvm-sh/nvm) the readme explains the 5 alternatives, but you could also just use `volta` (rustBTW).
- Use [`bass`](https://github.com/edc/bass) for interoperability.

Additionally, if you don't want a complex config, you can use the command `fish_config` which will give you a web-ui for customizing: themes, prompt, functions, and keybindings in a jiffy. This lets you get up and running with fish even faster.

## General practices

All my fish configs are written in such a way that I make no assumptions about what is installed. This is both so that each machine can be using all or none of my preferred apps and I still get access to my fish config. Before I use any command/app/tool, I check that it exists.

```bash
if type -q some_command
    # Configure some_command
end
```

## My configuration

I got tired of having to be careful not to commit things that were appended to my fish config file, so I decided I'd move it to the `conf.d` directory.

```
from: $HOME/.config/fish/config.fish
to:   $HOME/.config/fish/config.d/config.fish
```

Turns out `conf.d` is loaded before the main config file anyways, so this changes basically nothing.

```bash
├── conf.d/
│   └── config.fish
└── config.fish # This file is gitignored
```

As a result now the default config file can remain as a local config file. Any tool that wants to append things can do so and I don't have to care.

At any time I can look at my local config and move some of those into a `conf.d` file.

### conf.d directory

This is a feature of Fish, so you don't need to source this folder or anything. This directory contains specific tool/app configurations. This keeps my config file more specific to shell configurations.

Generally, if a tool appends a blob of code to my config file, that's a good candidate for something to go into a separate conf.d file. So another way of viewing files besides config.fish, is that they're all code-gen'd. Where as config.fish is code I've written.

### functions

Another great feature of fish, each file named after a command is a sort of lazy-evaluated script. All files here are only loaded when the function is called. So e.g. if I never uses `bass`, it'll never be evaluated.
