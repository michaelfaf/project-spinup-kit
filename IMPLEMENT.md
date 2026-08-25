# IMPLEMENT — Project Spin-Up System

> **AI: this file is addressed to you.** You are installing a project-management convention for your user, adapted to their environment. Work through the phases in order. Check off every task in `INSTALL-STATUS.md` as you complete it, and record every decision in its Decisions table — that file is how a future session resumes if this one ends. A human following along without an AI can do every step by hand.

## What you are installing

The system defined normatively in `SYSTEM.md` — a convention where every project is a folder with a tiny, fixed file set:

| File | Job | Updated when |
|---|---|---|
| `README.md` | Identity — what/why/how it works, standing rules, locked decisions | Scope or a decision changes. Never holds state. |
| `STATUS.md` | **Current state only** — a `State:` line, dated position lines, the board, waiting-ons, recurring work, inbox, blockers | Every session retire (plus marker/inbox writes, `SYSTEM.md` §4) |
| `History/LOG.md` | Session log, chronological, append-only | Every session retire (append) |
| `History/LESSONS.md` | Append-only lessons (what happened / why / rule going forward) | When a lesson lands |
| `History/Decisions/` | Numbered decision briefs — a tucked-away historical record, pointed at from README's Locked decisions | When a decision needs research |
| `NEXT-CHAT.md` | Handoff — the 2–4 lines the next session can't get from STATUS | At retire while a multi-session push (a push: one unit of outcome, `SYSTEM.md` §3) is live; deleted when the push ends |

…plus workspace-level files (`PORTFOLIO.md`, a general `LESSONS.md`, and a copy of `SYSTEM.md`), a session protocol — enter → plan → gate → execute → gate → retire, specified in `SYSTEM.md` §4, with a lightweight "touch" class for one-task asks — and a weekly review (`SYSTEM.md` §8) so nothing rots unseen.

The core discipline is the **STATUS test** and the **one-fact rule** (`SYSTEM.md` §1–§2): identity in README, state in STATUS, everything else in History; one fact lives in exactly one file, and everything else points at it.

---

## Phase 0 — Environment scan

Before asking the user anything, find out what you're working with. Check each and note findings in `INSTALL-STATUS.md` → Scan results:

1. **What am I?** Identify your own platform (Claude Code / Codex / Cursor / Cline / chat-only / other) and whether you can: create files, run shell commands, spawn subagents, persist memory across sessions.
2. **Where does the user's work live?** Look for an existing workspace: an Obsidian vault, a `~/projects`-style folder, a notes app export, a git repo of documents. Ask if nothing is visible from where you're running.
3. **Existing conventions?** If they already have project folders, READMEs, or an instructions file (`CLAUDE.md`, `AGENTS.md`, custom instructions), read them — you will *merge into* their conventions, not bulldoze them.
4. **Git available?** (`git --version`) — when available, git is on by default: it is the system's undo (`SYSTEM.md` §10).
5. **Already installed?** Look for an existing `(PROJECT TEMPLATE)/`, a `PORTFOLIO.md`, a workspace `SYSTEM.md`, or a convention block in the instructions file. Any found → this is an **upgrade, not a fresh install**: diff and merge rather than duplicate, pre-fill the Decisions table from what's already there, and re-ask only what's genuinely open.

If you cannot see the user's file system (chat-only): you'll run the human-driven path — you narrate, they create. Everything below still applies; `INSTALL-STATUS.md` lives wherever they can paste it back to you.

---

## Phase 1 — Decisions

Present each decision point conversationally: context, options with trade-offs, your recommendation (adjusted by scan findings). Record each choice in `INSTALL-STATUS.md` before moving on. Legal values live in `SYSTEM.md` §10; the conversation lives here.

### DP-1 — Where do projects live?

- **A. Inside their existing workspace/vault** — everything in one place; AI sessions see projects alongside notes. *Trade-off:* conventions must coexist with what's already there.
- **B. A dedicated `Projects/` folder** — clean start, easy to version. *Trade-off:* one more place to look.
- **C. Chat-managed (no file system)** — user keeps STATUS in a note and pastes it each session. *Trade-off:* most friction; only choose if there's genuinely no file access.

**Recommendation:** A if a workspace exists — meaning an organized folder or vault the user actually works in, not just loose files (loose files → B).

### DP-2 — Quality gates?

The system runs two checks — a **Plan Check** after planning (challenge the premise first, correctness last) and a **Ship Check** after execution (evidence per "Done means" line, then "will we have to redo this?"). Fresh eyes are the point; checklists, verdict tokens, and the failure path live in `SYSTEM.md` §5.

- **A. Subagent gates** — fresh-context subagents that never saw the working conversation run the checks. Strongest. *Requires* a platform that can spawn subagents (your scan knows).
- **B. Self-review gates** — you run both checks yourself, verbatim against `SYSTEM.md` §5. Weaker (you grade your own homework) but free.
- **C. No gates** — fastest; mistakes surface later.

**Recommendation:** A if the platform supports subagents; otherwise B. Choose C only for a user who wants minimum ceremony. Write the installed rule capability-conditionally ("a subagent if this platform has them, otherwise a clean self-review") so it survives a platform switch.

### DP-3 — Where do archived projects go?

When a project reaches `State: done` it is archived (`SYSTEM.md` §7) — final LOG entry, lessons harvested, folder moved out of the active set.

**Recommendation:** `Archive/<YYYY>/` inside wherever projects live (DP-1). Any stable folder works; year-bucketing keeps it navigable.

### DP-4 — Weekly review day?

A 10-minute AI-run pass over the portfolio and every active project (`SYSTEM.md` §8): stale projects, old blockers, missed chase dates, orphaned handoffs, pending promotions.

**Recommendation:** a fixed end-of-week slot. Pick the weekday; put it in the instructions wiring.

### DP-5 — Where do general lessons land?

Lessons true beyond one project (`SYSTEM.md` §6) need a home outside it.

- **A. Workspace `LESSONS.md`, with standing ones promoted into the instructions file (recommended)** — auditable list, instructions file stays lean.
- **B. Instructions file directly** — one less file. *Trade-off:* the instructions file bloats and there's no queue to review.

**Recommendation:** A. Cap what's copied into the instructions file; prune at the weekly review.

### Defaults — confirm, don't deliberate

State these briefly; vary only if the user pushes back (`SYSTEM.md` §10): light depth per project (files grow when earned) · History = LOG + LESSONS, briefs when a decision first needs research · handoff = `NEXT-CHAT.md` (manual paste under DP-1 C) · git on when available, with a retire commit.

---

## Phase 2 — Install

1. Create the template folder at the location from DP-1, named `(PROJECT TEMPLATE)/`, containing the files from `templates/` — use the table in `templates/README.md` for which files install and what each is renamed to (e.g. `PROJECT-README.md` → `README.md`). Adapt wording minimally — only rename terms that clash with vocabulary the user already uses; otherwise copy as-is.
2. Create the workspace files beside the projects: `PORTFOLIO.md` (from `templates/PORTFOLIO.md`, one row per existing project if any), an empty workspace `LESSONS.md` (DP-5 = A), `REVIEW.md` (the weekly-review format reference), and a copy of `SYSTEM.md` **minus its §12** — the spec and its rituals must survive this kit and be readable by any AI that opens the workspace.
3. **Wire it into your own standing instructions** — this is the step that makes it stick. Add to the user's instructions file (`CLAUDE.md`, `AGENTS.md`, custom instructions — whatever your platform reads every session), merged into what's already there. *Watch scope:* some platforms' instruction files are workspace-scoped (e.g. Cursor rules apply only when that folder is open) — place the file where the projects live, and tell the user the convention fires only there:
   - "The rules live in `SYSTEM.md` (workspace root). Files are authoritative — if platform memory conflicts with them, the file wins."
   - "New project → copy `(PROJECT TEMPLATE)/`, fill in README + STATUS."
   - "One-task ask → touch: do it, tick the board, no ceremony. Anything more → full session per `SYSTEM.md` §4."
   - "Working session in a project folder → enter per §4 (marker, staleness, inbox), retire through the §4 close-out checklist."
   - The gate rule per DP-2, written capability-conditionally.
   - "Every <DP-4 day>: run the weekly review per `SYSTEM.md` §8."
4. If git is available: init/commit (default on).
5. Tick Phase 2 in `INSTALL-STATUS.md`.

## Phase 3 — First live spin-up (the real test)

Ask the user for one real project — something actually on their plate. Intake, conversationally: name · what should an AI be able to *do* from this folder · what inputs exist to pull in · what done looks like · any guardrails. Then:

1. Copy the template, fill README (identity, standing rules) and STATUS (`State: active`, a board with 1–3 pushes, each with a one-line binary **"Done means:"** written *before* execution).
2. Run one working session on it end to end — enter, plan, [gate], execute a first task, [gate], **retire properly** through the full §4 close-out checklist, including the PORTFOLIO row.
3. Run the cold-read test (`SYSTEM.md` §11) **genuinely fresh**: a fresh-context subagent — or, without subagents, a brand-new chat — gets only the text of README + STATUS and must answer 4/4: what the project is, current position, next action, blockers. Self-grading by this session does not count. Show the user the result.

## Phase 4 — Wrap up

- Walk the user through what got installed and where (one short list).
- Tick everything remaining in `INSTALL-STATUS.md`, record final state.
- Leave them the habit in one line: **"Every session: enter by reading STATUS, retire by updating it."**
- Optional: delete this kit repo — the system now lives in their workspace, including its `SYSTEM.md`. (Keep the kit if they want to re-run or share it.)

---

## If things go wrong

- **Scan finds no file access and user expected it** → check platform permissions/settings first (that's usually it), else fall back to DP-1 option C.
- **User's existing conventions conflict** (e.g. they already have a STATUS-like file) → keep *their* names, map the jobs onto their files. The jobs (identity / current state / history) matter; the filenames don't.
- **A step fails twice** → don't loop. Note it in `INSTALL-STATUS.md` under Blockers, pick the nearest fallback, and tell the user what you did.
