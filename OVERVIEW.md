# Understanding the Project Spin-Up System

Read this before installing. It explains what the system is, every concept you'll meet, what a working session looks like, what gets installed, and what your job is versus your AI's. `IMPLEMENT.md` is the how; this is the what and why. This document *explains* the system — the normative rules live in [SYSTEM.md](SYSTEM.md).

## The problem

AI assistants have no memory between chats. If you run real, ongoing projects with one, you hit the same four walls:

1. **Every session starts from zero.** You re-explain the project, paste old context, and burn the first ten minutes reconstructing where you were.
2. **Work strands in dead chats.** The plan, the half-finished task, the decision you made — all live in a conversation that compacted, hit its limit, or you simply can't find.
3. **Decisions get re-litigated.** The AI re-asks questions you settled weeks ago, or worse, quietly decides them differently this time.
4. **Nothing compounds.** Lessons learned in March are gone by April. Every project is groundhog day.

The fix is not a bigger context window or a memory feature — those are invisible and unauditable. The fix is a convention: **the project's memory lives in files, in the project's own folder, in a shape any AI session (or human) can load in one read.**

## The core idea

Every project is a folder with a small, fixed file set, and every AI session follows the same ritual: **enter by reading the state, work, exit by writing the state back.** That's the whole system. Everything below is the detail that makes it hold up in practice.

## The concepts

**The three jobs: identity, state, history.** Every piece of project information has exactly one of three jobs, and each job has exactly one home:

- **`README.md` — identity.** What the project is, why it exists, how it works, its standing rules and locked decisions. Changes only when scope or a decision changes. Never holds current state.
- **`STATUS.md` — current state, only.** A dated position line ("where we are as of…"), a task board, and blockers. Updated at every session exit. This is the file a fresh session reads to know exactly where things stand.
- **`History/` — everything that already happened.** A rolling session log (`LOG.md`, newest first) and an append-only lessons file (`LESSONS.md`).

**The one-file rule.** One fact lives in exactly one file; everything else points at it. Duplicate a fact and the copies will disagree within a month.

**The STATUS test.** The discipline that keeps STATUS.md readable is a single question applied constantly: *"Would an agent loading current state need this paragraph?"* No → it moves to History. A STATUS file that scrolls is a STATUS file that's failed.

**The session protocol.** Every working session on a project has the same shape:
1. **Enter** — read README + STATUS (and the parent project's, if nested). Now the AI knows the project cold.
2. **Plan** — what this session will do, with each push carrying a binary **"Done means:"** line written *before* execution (so "done" is checkable, not vibes).
3. **Gate** (optional, your install decision) — a plan check before executing.
4. **Execute** — work the board.
5. **Gate** — a ship check after executing.
6. **Retire** — update STATUS's dated position, tick the board, append the session to LOG, and write the handoff. A session that skips retirement strands its work — retiring properly is the non-negotiable step.

**"Done means:" lines.** Every push on the board gets one line stating what done looks like, in binary, testable terms — written before the work starts. This is what makes the ship check possible and stops "mostly done" from living on a board for weeks.

**Quality gates (Plan Check and Ship Check).** Two checks run by fresh eyes — ideally a subagent that never saw the working conversation, otherwise the AI reviewing itself against a fixed checklist. The Plan Check challenges the premise first ("is there a simpler way to get this outcome?") before checking correctness. The Ship Check verifies each "Done means:" line against actual output — not memory — and asks "will any of this have to be redone?" How strong your gates are is one of the five install decisions.

**The handoff (`NEXT-CHAT.md`).** On retirement, the session writes the opening prompt for the *next* session into the project folder, and the next session starts by reading it. The principle: **prompts live on disk, not in chat — the chat can die; the work can't.** Deleted when the push ends.

**Lessons.** 2–4 lines each, appended to `History/LESSONS.md` when something teaches you something: what happened, why, the rule going forward. A lesson that should change future behavior gets *promoted* — with your sign-off — into the project README's standing rules, where every future session will obey it. This is the compounding loop: projects get smarter, not just older.

**Decision briefs (optional third tier).** When decisions start needing real research, each gets a numbered brief ending in a *lean, never a ruling* — the human rules. Most people add this tier later, when it's earned.

**The template folder.** One `(PROJECT TEMPLATE)/` folder in your workspace containing the file set. Spinning up a new project = copy it, fill in README and STATUS. Ten minutes, same shape every time.

**The instructions-file wiring.** One short block added to your AI's standing instructions (CLAUDE.md, AGENTS.md, custom instructions — whatever it reads every session): new project → copy the template; entering a project folder → read README + STATUS; exiting → update STATUS, append LOG, write the handoff. This is the step that makes the system automatic instead of a thing you have to remember to ask for.

## What a week with this looks like

Monday you open a chat: "let's work on the pricing project." The AI reads that folder's README + STATUS, says "we're mid-push on the tier comparison, next task is X, one blocker" — no re-explaining. You work. At the end it updates STATUS, logs the session, writes NEXT-CHAT.md. Wednesday a *different* chat — or a different AI entirely — opens the same folder and picks up in one read. Friday you start something new: copy the template, ten minutes of intake, and the new project is born already carrying the convention. A month in, the lessons file has caught two mistakes you'd otherwise have repeated.

## What gets installed, and your role

Install is five decisions (where projects live, default depth per project, gate strength, handoff mechanism, how much history), then three artifacts: the template folder, the instructions-file wiring, and one real project spun up and taken through a full session — enter to retire — as the live test. The install is verified the only way that matters: a fresh session must be able to open the project cold and state exactly where it stands, from README + STATUS alone.

**Your job, ongoing:** rule on promotions (lesson → standing rule), make the decisions the briefs lean on, and actually work the projects. **The AI's job:** everything mechanical — reading state on entry, keeping STATUS true, logging, handoffs, running the gates.

**What you need:** nothing beyond an AI and somewhere to put folders. File-capable AI platforms (Claude Code, Cursor, Codex, Cline…) run everything automatically; chat-only AIs work on the manual path (you create files, paste STATUS back each session); the system even works with no AI at all — it's a paper-compatible convention.

## What it is not

- Not project-management software — no app, no accounts, no sync. Plain markdown and a habit.
- Not a note-taking method — it deliberately manages *state*, not knowledge.
- Not tied to any AI vendor — the files are the system; any assistant that can read them can run it.

It was built and refined in daily use running a real company's projects with AI assistants — the conventions here are the survivors, not the theory.
