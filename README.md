# Project Spin-Up Kit

A field-tested operating system for running projects with an AI assistant: a small set of files (`README` / `STATUS` / `History`) plus a session protocol (plan → gate → execute → gate → retire) that lets any AI session pick up a project cold and leave it clean. Built and refined in daily use inside a real business; this kit adapts it to *your* setup instead of copying someone else's.

## How to use this kit

**If you use an AI coding assistant** (Claude Code, Codex, Cursor, Cline, Copilot, etc.):

1. Clone or download this repo.
2. Open it with your assistant and say: **"Read IMPLEMENT.md and walk me through setting this up."**
3. Your AI will scan your setup, walk you through a handful of decisions, install the templates where they belong, and track progress in `STATUS.md`. Recommended model: the most capable one you have access to, at high reasoning effort (e.g. Claude Fable 5 high, or Claude Opus high).

**If you use a chat AI** (claude.ai, ChatGPT) without file access:

1. Paste the contents of `IMPLEMENT.md` into a chat.
2. Say: "Walk me through this. I'll create the files by hand as you go."
3. Keep `STATUS.md` yourself (a note or doc) and paste it back at the start of each new chat.

**If you have no AI assistant:**

Read `IMPLEMENT.md` yourself — every step is doable by hand. The decision points tell you the trade-offs; the `templates/` folder has every file you'll create.

## What's in here

| File | What it is |
|---|---|
| `IMPLEMENT.md` | The full setup walkthrough (written for your AI to execute, readable by humans) |
| `STATUS.md` | Progress checklist — your AI checks things off and records your decisions here |
| `AGENTS.md` / `CLAUDE.md` | Entry points so coding agents auto-orient when opening this repo |
| `templates/` | The project file templates you'll install |

## What you end up with

- A project-template folder in your workspace your AI copies for every new project
- A convention: identity in `README.md`, current state in `STATUS.md`, everything else in `History/`
- A session protocol your AI follows: enter with context, exit with a clean handoff — so no chat session ever strands your work
