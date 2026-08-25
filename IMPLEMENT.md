# IMPLEMENT — Project Spin-Up System

> **AI: this file is addressed to you.** You are installing a project-management convention for your user, adapted to their environment. Work through the phases in order. Check off every task in `STATUS.md` as you complete it, and record every decision in its Decisions table — that file is how a future session resumes if this one ends. A human following along without an AI can do every step by hand.

## What you are installing

The system defined normatively in `SYSTEM.md` — a convention where every project is a folder with a tiny, fixed file set:

| File | Job | Updated when |
|---|---|---|
| `README.md` | Identity — what/why/how it works, standing rules, locked decisions | Scope or a decision changes. Never holds state. |
| `STATUS.md` | **Current state only** — dated position lines, a task board, blockers | Every session retire |
| `History/LOG.md` | Rolling session log, newest first | Every session retire (append) |
| `History/LESSONS.md` *(if DP-5 = B or C)* | Append-only lessons (what happened / why / rule going forward) | When a lesson lands |
| `NEXT-CHAT.md` *(if DP-4 = A)* | Handoff — the opening prompt for the next session | At retire while a multi-session push (a push: one unit of outcome, `SYSTEM.md` §3) is live; deleted when the push ends |
| Decision briefs *(if DP-5 = C)* | One numbered brief per researched decision, ending in a *lean, never a ruling* | When a decision needs research |

…plus a session protocol — enter → plan → gate → execute → gate → retire, specified in `SYSTEM.md` §4: every session **enters** by reading README + STATUS and **retires** by writing the state back, so no project ever depends on a chat that died.

The core discipline is the **STATUS test** and the **one-fact rule** (`SYSTEM.md` §1–§2): identity in README, state in STATUS, everything else in History; one fact lives in exactly one file, and everything else points at it.

---

## Phase 0 — Environment scan

Before asking the user anything, find out what you're working with. Check each and note findings in `STATUS.md` → Scan results:

1. **What am I?** Identify your own platform (Claude Code / Codex / Cursor / Cline / chat-only / other) and whether you can: create files, run shell commands, spawn subagents, persist memory across sessions.
2. **Where does the user's work live?** Look for an existing workspace: an Obsidian vault, a `~/projects`-style folder, a notes app export, a git repo of documents. Ask if nothing is visible from where you're running.
3. **Existing conventions?** If they already have project folders, READMEs, or an instructions file (`CLAUDE.md`, `AGENTS.md`, custom instructions), read them — you will *merge into* their conventions, not bulldoze them.
4. **Git available?** (`git --version`) — versioning is optional but recommended.

If you cannot see the user's file system (chat-only): you'll run the human-driven path — you narrate, they create. Everything below still applies; `STATUS.md` lives wherever they can paste it back to you.

---

## Phase 1 — Decisions

Present each decision point conversationally: context, options with trade-offs, your recommendation (adjusted by scan findings). Record each choice in `STATUS.md` before moving on.

### DP-1 — Where do projects live?

**Options:**
- **A. Inside their existing workspace/vault** — everything in one place; AI sessions see projects alongside notes. *Trade-off:* conventions must coexist with what's already there.
- **B. A dedicated `Projects/` folder (optionally a git repo)** — clean start, easy to version. *Trade-off:* one more place to look.
- **C. Chat-managed (no file system)** — user keeps STATUS in a note and pastes it each session. *Trade-off:* most friction; only choose if there's genuinely no file access.

**Recommendation:** A if a workspace exists — meaning an organized folder or vault the user actually works in, not just loose files (loose files → B). 

**Sub-decision, all options: version projects with git?** Recommendation: yes if the scan found git; skip without guilt if the user doesn't use it.

### DP-2 — Default depth per project?

**Options:**
- **A. Light (recommended):** `README.md` + 5-line `STATUS.md`, empty `History/`. Files grow only when the work earns it.
- **B. Full from day one:** all files pre-created (except `NEXT-CHAT.md` and decision briefs, which are only ever created when the work calls for them). *Trade-off:* scaffolding for projects that may die young; empty files rot.

**Recommendation:** A. Depth scales; nothing grows until the work earns it.

### DP-3 — Quality gates?

The system runs two checks — a **Plan Check** after planning (challenge the premise first, correctness last) and a **Ship Check** after execution (verify each "Done means" line, then "will we have to redo this?"). Fresh eyes are the point; both checklists live verbatim in `SYSTEM.md` §5.

**Options:**
- **A. Subagent gates** — fresh-context subagents that never saw the working conversation run the checks. Strongest. *Requires* a platform that can spawn subagents (your scan knows).
- **B. Self-review gates** — you run both checks yourself, verbatim against the checklists in `SYSTEM.md` §5. Weaker (you grade your own homework) but free.
- **C. No gates** — fastest; mistakes surface later.

**Recommendation:** A if your platform supports subagents; otherwise B. Choose C only for a user who wants minimum ceremony.

### DP-4 — Session handoff mechanism?

How does the *next* session pick up where this one stopped?

**Options:**
- **A. `NEXT-CHAT.md` file** — at retire, while a multi-session push is live, write the opening prompt for the next session into the project folder; delete it when the push ends. Works on every file-capable platform.
- **B. Platform memory** — if your platform has persistent cross-session memory, STATUS + memory may suffice. *Trade-off:* memory is invisible and unauditable; files are checkable.
- **C. Manual paste** — chat-only users paste STATUS at session start.

**Recommendation:** A. "Prompts live on disk, not in chat — the chat can die; the work can't."

### DP-5 — How much History?

**Options:**
- **A. LOG only** — one rolling session log. Simplest.
- **B. LOG + LESSONS (recommended)** — lessons are the compounding asset; format and promotion rule in `SYSTEM.md` §6.
- **C. Full: LOG + LESSONS + Decision Briefs** — researched decisions each get a numbered brief (template: `templates/DECISION-BRIEF.md`) ending in a *lean, never a ruling* (the user rules). Add this tier when decisions start needing real research.

**Recommendation:** B now; graduate to C when it's earned.

---

## Phase 2 — Install

1. Create the template folder at the location from DP-1, named `(PROJECT TEMPLATE)/`, containing the files from `templates/` that match the DP-4/DP-5 choices — use the table in `templates/README.md` for which files install and what each is renamed to (e.g. `PROJECT-README.md` → `README.md`). DP-2 governs how much of it each new project starts with, not which files belong to the install. Adapt wording minimally — only rename terms that clash with vocabulary the user already uses; otherwise copy as-is.
2. **Wire it into your own standing instructions** — this is the step that makes it stick. Add to the user's instructions file (`CLAUDE.md`, `AGENTS.md`, custom instructions — whatever your platform reads every session), merged into what's already there. *Watch scope:* some platforms' instruction files are workspace-scoped (e.g. Cursor rules apply only when that folder is open) — place the file where the projects live, and tell the user the convention fires only there:
   - "New project → copy `(PROJECT TEMPLATE)/`, fill in README + STATUS."
   - "Working in a project folder → on entry read its README + STATUS (and the parent's, if nested); on retire (when you finish working the folder) update STATUS, append LOG, [write NEXT-CHAT.md]." *(bracket per DP-4)*
   - The gate rule per DP-3.
3. If git was chosen (DP-1 sub-decision): init/commit.
4. Tick Phase 2 in `STATUS.md`.

## Phase 3 — First live spin-up (the real test)

Ask the user for one real project — something actually on their plate. Intake, conversationally: name · what should an AI be able to *do* from this folder · what inputs exist to pull in · what done looks like · any guardrails. Then:

1. Copy the template, fill README (identity, standing rules) and STATUS (a board with 1–3 pushes, each with a one-line binary **"Done means:"** written *before* execution).
2. Run one working session on it end to end — enter, plan, [gate], execute a first task, [gate], **retire properly**: update STATUS's dated position line, tick the board, append LOG, write the handoff (DP-4).
3. Show the user the retired folder. The system is installed only when a *fresh* session can open that folder cold and know exactly where things stand — verify by summarizing the project *only* from README + STATUS.

## Phase 4 — Wrap up

- Walk the user through what got installed and where (one short list).
- Tick everything remaining in `STATUS.md`, record final state.
- Leave them the habit in one line: **"Every session: enter by reading STATUS, retire by updating it."**
- Optional: delete this kit repo — the system now lives in their workspace. (Keep it if they want to re-run or share it.)

---

## If things go wrong

- **Scan finds no file access and user expected it** → check platform permissions/settings first (that's usually it), else fall back to DP-1 option C.
- **User's existing conventions conflict** (e.g. they already have a STATUS-like file) → keep *their* names, map the jobs onto their files. The jobs (identity / current state / history) matter; the filenames don't.
- **A step fails twice** → don't loop. Note it in STATUS under Blockers, pick the nearest fallback, and tell the user what you did.
