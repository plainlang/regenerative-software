<h1 align="center">Regenerative software</h1>

<p align="center">Regeneration lets your coding agent test and fix its understanding of what to build, so it never loses your trust.</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@plainlang/regenerative-software"><img src="https://img.shields.io/npm/v/%40plainlang%2Fregenerative-software" alt="npm" /></a>
  <a href="https://www.plainlang.org/docs/"><img src="https://img.shields.io/badge/docs-plainlang.org-blue" alt="docs: plainlang.org" /></a>
  <img src="https://img.shields.io/badge/license-MIT-blue" alt="license: MIT" />
</p>

This repo is the entry point to the [regenerative software stack](#the-regenerative-software-stack), which runs inside the coding agent you already use. You and your agent keep refining your shared understanding of the system into durable system knowledge. It lives in [`***plain`](https://www.plainlang.org), a structured language that reads like prose. The agent regenerates the software from that knowledge, so every change starts there instead of in the code.

## Why regenerate

Regeneration makes the agent's understanding something you can test.

Without it, an agent whose context is cleared has only the code, which says what the system does, not what it must do. So it guesses, and every change it builds on a guess makes it worse.

With it, the software is regenerated from durable system knowledge every time, so what it builds shows whether the agent understood. Where it did not, the gap traces back to the knowledge, and you fix that, not the code.

Delete the code and the agent can rebuild it. Delete the knowledge and nobody can.

## Install

One command adds the stack to the agent you already use:

```bash
npx @plainlang/regenerative-software@latest install
```

Requires *Node.js 22* or newer and *Python 3.11* or newer.

Supports *Claude Code*, *Codex*, *GitHub Copilot*, *OpenCode* and *ForgeCode*. Options, updating and uninstalling are in the [plain-forge guide](https://github.com/plainlang/plain-forge#getting-started).

## The regenerative software stack

Installed into your agent:

- **[plain-forge](https://github.com/plainlang/plain-forge)**: the agent skills that work with you to write, check and update your durable system knowledge in `.plain` files.
- **[pyro](https://github.com/plainlang/pyro)**: the *renderer* skill. Your agent regenerates working, tested code from `.plain` files itself, with a few helper scripts in Python. Bundled with `plain-forge`.
- **[plain-parser](https://github.com/plainlang/plain-parser)**: validates a `.plain` file's structure before it renders. `--skip-plain-parser` leaves it out.

Worth adding yourself:

- **[plyn](https://github.com/plainlang/plyn)**: highlighting, hover, go-to-definition, rename and diagnostics for `.plain` files in VS Code, Cursor and Zed.
- **[plainlang-examples](https://github.com/plainlang/plainlang-examples)**: example `.plain` projects, including hello-world, Pong and a task manager.

The rest is open source too: the grammar, the language server and the source of the `***plain` documentation all live under [github.com/plainlang](https://github.com/plainlang).

## Your first session

Start your agent in an empty directory and tell it:

```
Use forge-plain to start a new ***plain project.
```

Describe something you would actually build. The agent works with you to write down durable system knowledge: what the system must do, its contracts and its acceptance tests. It asks one question at a time, including which language to render in, and writes each answer into a `.plain` file as you go, so ambiguities surface in the conversation instead of later in the code. When the healthcheck passes, the agent names the file to render:

```
/render-spec <file>.plain
```

A render passes only when every test passes, and it works the same on a `.plain` project you already have. The code is yours, in your language, with no dependency on this stack.

Every later change goes the same way: describe a feature or report a bug, the agent updates the `.plain` file, then render again. You never patch the code by hand. The full workflow is in the [plain-forge usage guide](https://github.com/plainlang/plain-forge#usage).

## Your .plain files

> The ***plain language reference lives at [plainlang.org](https://www.plainlang.org/docs/).

A `.plain` file is a persistent document that grows with what you and your agent understand about the system.

The structure is light, and it carries semantics, not styling:

- A `:concept:` is a defined term that the file, its imports and the renderer all resolve to the same thing.
- The section headers, such as `***implementation reqs***` and `***functional specs***`, separate implementation requirements from the behaviour to test.
- Frontmatter imports pull in shared modules with their own concepts and tests.
- A bullet under acceptance tests is a test every render must pass.

The format is standardized, so it can be checked automatically and shared across a team instead of duplicated. This is what hello-world looks like:

```plain
---
description: '"hello, world" in Plain (Python version)'
import:
  - python-console-app-template
---

***implementation reqs***

- :MainExecutableFile: of :App: should be called "hello_world.py".

***functional specs***

- Display "hello, world"

  ***acceptance tests***

  - :App: should exit with status code 0 indicating successful execution.

  - :App: should complete execution in under 1 second.
```

The import is a shared module that defines `:App:` and how it is tested, so this file only says what makes hello-world different.

## Running it in production

The open source stack is complete on its own. When regenerative software moves into production, [∗codeplain](https://codeplain.ai) runs regeneration safely and reliably across your staging and production environments. Try it at [platform.codeplain.ai](https://platform.codeplain.ai) or [book a demo](https://codeplain.ai/book-a-demo).

## Status

> **Work in progress.** The install CLI and its flags, skill names, and `.plain` syntax can all change between releases. Each release's changes are on the [plain-forge releases page](https://github.com/plainlang/plain-forge/releases) and in [pyro's changelog](https://github.com/plainlang/pyro/blob/main/CHANGELOG.md). Tell us what broke, and bring questions and ideas, by [opening an issue](https://github.com/plainlang/regenerative-software/issues).

## License

MIT. Linked repos carry their own licenses. Code you render from your `.plain` files is yours.
