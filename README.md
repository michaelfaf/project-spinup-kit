# Project Spin-Up Kit

A field-tested operating system for running projects with an AI assistant: a small set of files per project (`README.md` / `STATUS.md` / a `History/` folder) plus a session protocol (enter → plan → gate → execute → gate → retire) that lets any AI session pick up a project cold and leave it clean. Built and refined in daily use inside a real business; this kit adapts it to *your* setup instead of copying someone else's.

## Read this first — what this is and why

**The problem.** AI assistants have no memory. Every new chat starts from zero: you re-explain the project, the AI re-asks questions you already answered, and work stranded in an old session quietly dies there. The more projects you run, the worse it compounds.

**What this installs.** A minimal operating system for projects. Every project gets the same three-part shape — `README.md` (what this project *is*), `STATUS.md` (where it stands right now), `History/` (what already happened) — plus a session ritual your AI follows every time it touches a project: read the state, plan, check the plan, execute, check the work, then retire cleanly so the next session can pick up cold.

**Why it works.** The project's memory lives in files, not in any chat. A chat can die, compact, or be three weeks old — the project doesn't care. Any AI session (or human) opens the folder and knows exactly where things stand and what's next.

**Use it if:** you run two or more ongoing projects with an AI assistant · you've lost work to a dead chat · you're tired of re-explaining context · you want anyone to open a project cold and know its state in one read.

**It is not** project-management software or tied to one AI vendor. It's plain markdown files and a habit.

**→ Want the full picture before deciding? Read [OVERVIEW.md](OVERVIEW.md)** — every concept, what a working session looks like, what gets installed, and your role vs your AI's, in about five minutes.

## How to use this kit

**If you use an AI coding assistant** (Claude Code, Codex, Cursor, Cline, Copilot, etc.):

1. Clone or download this repo.
2. Open it with your assistant and say: **"Read IMPLEMENT.md and walk me through setting this up."**
3. Your AI will scan your setup, walk you through a handful of decisions, install the templates where they belong, and track progress in `INSTALL-STATUS.md`. Recommended model: the most capable one you have access to, at high reasoning effort (e.g. Claude Fable 5 high, or Claude Opus high).

**If you use a chat AI** (claude.ai, ChatGPT) without file access:

1. Paste the contents of `IMPLEMENT.md` (and `SYSTEM.md`, the rules it installs) into a chat.
2. Say: "Walk me through this. I'll create the files by hand as you go."
3. Keep `INSTALL-STATUS.md` yourself (a note or doc) and paste it back at the start of each new chat.

**If you have no AI assistant:**

Read `IMPLEMENT.md` and `SYSTEM.md` yourself — every step is doable by hand. The decision points tell you the trade-offs; the `templates/` folder has every file you'll create.

## What's in here

| File | What it is |
|---|---|
| `OVERVIEW.md` | The full explanation — read this first to understand the system before installing |
| `SYSTEM.md` | The normative spec — the system's rules in one place; where documents disagree, it wins |
| `IMPLEMENT.md` | The full setup walkthrough (written for your AI to execute, readable by humans) |
| `INSTALL-STATUS.md` | Install-progress checklist — your AI checks things off and records your decisions here |
| `AGENTS.md` / `CLAUDE.md` | Entry points so coding agents auto-orient when opening this repo |
| `templates/` | The project file templates you'll install |

## What you end up with

- A project-template folder in your workspace your AI copies for every new project
- A convention: identity in `README.md`, current state in `STATUS.md`, everything else in `History/`
- A session protocol your AI follows: enter with context, retire through a checklist — so no chat session ever strands your work
- A portfolio file and a weekly AI-run review, so nothing across your projects rots unseen
- The system's rulebook (`SYSTEM.md`) installed in your workspace, readable by any AI you point at it
