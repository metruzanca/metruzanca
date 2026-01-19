---
timestamp: 2026-01-15T16:45:00
publish: false
tags:
  - ai
  - git worktrees
  - claude code
title: How I use worktrees with coding agents.
description: Git worktrees can somethings be difficult to work with but they heavily simplify parallelization of coding agents. (Claude Code, OpenCode or any other tools)
canonical_url: https://zanca.dev/blog/worktrees-for-agents
---

> [!tldr]
> Use git worktrees to parallelize coding agents across different branches. To streamline setup, use [mise](https://mise.jdx.dev/) for language-agnostic runtime and dependency management—create a `.mise.toml` file with `[hooks] postinstall = []` to auto-run setup after `mise install` in each new worktree. For advanced worktree management, check out [worktrunk.dev](https://worktrunk.dev) by [@max_sixty](https://x.com/max_sixty).

Want to have multiple coding agents working on different features simultaneously without them stepping on each other? Git worktrees are the answer, but they come with some overhead that's actually an opportunity in disguise.

I've been using coding agents like Claude Code and OpenCode more frequently, and I quickly ran into the problem of wanting to parallelize their work. You can't have one agent working on a feature branch while another is making changes to the main branch in the same workspace—you'll end up with conflicts, polluted working directories, and a whole lot of frustration.

## Git Worktrees: The Solution

Git worktrees let you have multiple working directories for the same repository. Each worktree is essentially its own checkout of your repo, but they all share the same `.git` directory. This means you can have one agent working in `../myproject.feature-a` while another works in `../myproject.feature-b`, completely isolated from each other.

The basic command is straightforward:

```bash
git worktree add <path> <branch>
```

### The Pros

**Isolated workspaces** mean each agent gets its own directory with no risk of conflicts. You get **true parallelization**—agents can work simultaneously without blocking each other. It's also **easy to see what each agent is working on** just by looking at your directory structure. Each worktree maintains its own **independent state**, so uncommitted changes in one won't affect the others.

### The Cons

The main downsides are **verbose git commands** (you'll need to specify worktree paths or use the `-C` flag), **repository setup overhead** (each worktree needs dependencies installed and environment configured), and **management complexity** (more directories to track and clean up).

The setup overhead is the one that really gets people. Having to install dependencies and configure the environment for each new worktree feels tedious, especially when you're spinning up multiple worktrees in quick succession.

## Turning Cons into Opportunities: Understanding Your Repo

Here's the thing though: that setup overhead isn't just a burden—it's forcing you to understand how your project actually works. When you have to set up a worktree from scratch, you're forced to think about what your project actually needs to run.

> This is actually valuable. How many times have you cloned a repo and had no idea how to get it running? Or worse, had it "just work" because of some global state you forgot about?

### Optimizing Setup with mise

This is where tools like [mise](https://mise.jdx.dev/) (formerly rtx) come in. Mise is a language-agnostic tool for managing runtime versions and tasks. Instead of relying on `package.json` scripts, `Makefile` targets, or `pyproject.toml` scripts, you can use a single `.mise.toml` or `.tool-versions` file that works across Python, JavaScript, Go, Rust, or whatever language you're using.

With mise, setting up a new worktree becomes:

```bash
git worktree add ../myproject.feature-a feature-a
cd ../myproject.feature-a
mise install  # Installs all runtime versions and runs postinstall hooks
```

I use the pattern `../$reponame.$branchname` for worktree locations. This keeps them organized and makes it clear which repo and branch each worktree represents.

I use mise's `postinstall` hook to automatically run setup tasks after installation, so I don't need a separate setup command. In my `.mise.toml`:

```toml
[hooks]
postinstall = [
  "npm install",  # or pip install, go mod download, etc.
  "npm run build",  # or whatever setup steps you need
]
```

Mise can handle:
- Runtime version management (Node, Python, Go, Rust, etc.)
- Dependency installation
- Environment setup
- Common tasks (test, build, lint, etc.)

The beauty of this approach is that it's **language agnostic**. Whether you're working with a Python project, a Go service, or a JavaScript app, the setup process is the same. It's **reproducible**—anyone (or any agent) can recreate the environment. It's **simplified**—one tool instead of juggling multiple language-specific solutions. And it's **agent-friendly**—a clear, scriptable setup process that's easy to automate.

### Example Workflow

Here's the complete workflow I use when spinning up a new worktree for an agent:

```bash
# 1. Create the worktree
git worktree add ../myproject.feature-a feature-a

# 2. Navigate to it and install dependencies
cd ../myproject.feature-a
mise install  # This runs postinstall hooks automatically

# 3. Open your coding agent of choice in this directory
# For Claude Code: Open the folder in Cursor/VS Code
# For OpenCode: Point it to this directory
# etc.
```

> Important: This workflow assumes you'll be deleting worktrees (and their directories) once the PR gets merged. After merging, clean up with `git worktree remove ../myproject.feature-a` or just delete the directory and run `git worktree prune` to clean up the reference.

> [!note]
> For JS developers: Since we delete worktrees when done, it's best to use tools like pnpm, bun, or deno, since they all use a global cache and either symlink `node_modules` or don't have `node_modules` at all. yarn can work if symlinks are enabled (not by default). My recommendation would be to use bun if able to, but pnpm is a great fallback and is generally the easiest to migrate to. Bun should be easy too but depending on your runtime usage/requirements it might need some extra setup (which you could just ask your agent to fix for you).

### Benefits of This Approach

I've found that using mise with worktrees has made my workflow much smoother. When I spin up a new worktree, I know exactly what needs to happen to get it running, and I can script it easily. More importantly, I understand my project's dependencies better because I've had to explicitly define them.

## Taking It Further: Worktrunk.dev

If you're managing many worktrees or want additional tooling around worktree management, check out [worktrunk.dev](https://worktrunk.dev). It's an even more optimized setup for worktree workflows, created by [@max_sixty](https://x.com/max_sixty). I haven't used it extensively yet, but it looks promising for anyone who's really leaning into the worktree workflow.

---

Worktrees enable true parallelization of agent work, and the "overhead" of setup is actually valuable—it forces better project understanding. Tools like mise make setup language-agnostic and reproducible, which is exactly what you need when working with multiple agents across different features.
