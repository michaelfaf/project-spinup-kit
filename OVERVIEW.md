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

Every project is a folder with a small, fixed file set, and every AI session follows the same ritual: **enter by reading the state, work, retire by writing the state back.** That's the whole system. Everything below is the detail that makes it hold up in practice.

## The concepts

**The three jobs: identity, state, history.** Every piece of project information has exactly one of three jobs, and each job has exactly one home:

- **`README.md` — identity.** What the project is, why it exists, how it works, its standing rules and locked decisions. Changes only when scope or a decision changes. Never holds current state.
- **`STATUS.md` — current state, only.** A dated position line ("where we are as of…"), a task board, and blockers. Updated at every retire. This is the file a fresh session reads to know exactly where things stand.
- **`History/` — everything that already happened.** A rolling session log (`LOG.md`, newest first) and an append-only lessons file (`LESSONS.md`).

**The one-file rule.** One fact lives in exactly one file; everything else points at it. Duplicate a fact and the copies will disagree within a month.

**The STATUS test.** The discipline that keeps STATUS.md readable is a single question applied constantly: *"Would an agent loading current state need this paragraph?"* No → it moves to History. A STATUS file that scrolls is a STATUS file that's failed.

**The session protocol.** Every working session on a project has the same shape:
1. **Enter** — read README + STATUS. Now the AI knows the project cold. (Projects don't nest — related work is a sibling project with a pointer.)
2. **Plan** — what this session will do, with each push (a push is one unit of outcome on the board) carrying a binary **"Done means:"** line written *before* execution (so "done" is checkable, not vibes).
3. **Gate** (optional — one install decision governs both gates) — the Plan Check, before executing.
4. **Execute** — work the board.
5. **Gate** (same install decision) — the Ship Check, after executing.
6. **Retire** — a binary close-out checklist (SYSTEM.md §4): position updated, board true, decisions landed, lessons captured, LOG appended, handoff handled, portfolio row refreshed. A session that skips retirement strands its work — retiring properly is the non-negotiable step.

**Touch sessions.** A one-task ask — check a status, fix a line — skips the ceremony: do it, tick the board, done. The moment a touch grows past one task, changes a decision, or hits a blocker, it converts to a full session. Naming the cheap path is what keeps the full protocol from eroding.

**"Done means:" lines.** Every push on the board gets one line stating what done looks like, in binary, testable terms — written before the work starts. This is what makes the ship check possible and stops "mostly done" from living on a board for weeks.

**Quality gates (Plan Check and Ship Check).** Two checks run by fresh eyes — ideally a subagent that never saw the working conversation, otherwise the AI reviewing itself against a fixed checklist. The Plan Check challenges the premise first ("is there a simpler way to get this outcome?") before checking correctness. The Ship Check verifies each "Done means:" line against actual output — not memory — and asks "will any of this have to be redone?" How strong your gates are is one of the five install decisions.

**The handoff (`NEXT-CHAT.md`).** On retirement — while a multi-session push is live — the session writes the 2–4 lines the next session *can't* get from STATUS ("start with…", "watch out for…") into the project folder. The principle: **prompts live on disk, not in chat — the chat can die; the work can't.** Deleted when the push ends; and if it ever disagrees with STATUS, STATUS wins.

**Lessons.** 2–4 lines each, appended to `History/LESSONS.md` when something teaches you something: what happened, why, the rule going forward. A lesson that should change future behavior gets *promoted* — with your sign-off — after one routing question: true only here, or everywhere? Project-specific lessons land in that project README's standing rules; general ones land in the workspace `LESSONS.md` and, when standing, your AI's instructions file. This is the compounding loop: *all* your projects get smarter, not just the one where the lesson happened.

**Decision briefs.** When a decision needs real research, it gets a numbered brief ending in a *lean, never a ruling* — the human rules. Briefs live tucked away in `History/Decisions/` as the historical record; the ruling itself lands as a row in the project README's Locked decisions, pointing back at the brief.

**The portfolio and the weekly review.** One workspace file — `PORTFOLIO.md`, a row per project: state, last touched, next action, waiting-on — answers "what's live, what's stalled, who am I waiting on" without opening folders. Once a week, on your chosen day, the AI runs a ten-minute review against it: projects untouched too long, stale STATUS files, old blockers, missed chase dates, orphaned handoffs, lessons waiting for your sign-off. Without it, a project nobody opens gives off no signal at all.

**The project lifecycle.** Every STATUS opens with a state — active, parked, waiting-external, or done. Done projects get archived: final log entry, lessons harvested, folder moved to the archive location. A year in, your projects folder holds only what's alive.

**The template folder.** One `(PROJECT TEMPLATE)/` folder in your workspace containing the file set. Spinning up a new project = copy it, fill in README and STATUS. Ten minutes, same shape every time.

**The instructions-file wiring.** One short block added to your AI's standing instructions (CLAUDE.md, AGENTS.md, custom instructions — whatever it reads every session): new project → copy the template; entering a project folder → read README + STATUS; retiring → update STATUS, append LOG, write the handoff (if your install uses one). This is the step that makes the system automatic instead of a thing you have to remember to ask for.

## What a week with this looks like

Monday you open a chat: "let's work on the pricing project." The AI reads that folder's README + STATUS, says "we're mid-push on the tier comparison, next task is X, one blocker" — no re-explaining. You work. At the end it updates STATUS, logs the session, writes NEXT-CHAT.md. Wednesday a *different* chat — or a different AI entirely — opens the same folder and picks up in one read. Friday you start something new: copy the template, ten minutes of intake, and the new project is born already carrying the convention. End of the week, the AI runs the ten-minute review: one project untouched for three weeks, one chase date missed — you rule, it fixes. A month in, the lessons files have caught mistakes you'd otherwise have repeated — in every project, not just where they happened.

## What gets installed, and your role

Install is five decisions (where projects live, gate strength, where archives go, the weekly review day, where general lessons land), then four artifacts: the template folder, the workspace files (portfolio, lessons, the `SYSTEM.md` rulebook), the instructions-file wiring, and one real project spun up and taken through a full session — enter to retire — as the live test. The install is verified the only way that matters: a genuinely fresh reader — one that has seen nothing but README + STATUS — must state exactly what the project is, where it stands, what's next, and what's blocked.

**Your job, ongoing:** rule on promotions (lesson → standing rule), make the decisions the briefs lean on, act on the weekly review's report, and actually work the projects. **The AI's job:** everything mechanical — reading state on entry, keeping STATUS true, logging, handoffs, running the gates.

**What you need:** nothing beyond an AI and somewhere to put folders. File-capable AI platforms (Claude Code, Cursor, Codex, Cline…) run everything automatically; chat-only AIs work on the manual path (you create files, paste STATUS back each session); the system even works with no AI at all — it's a paper-compatible convention.

## What it is not

- Not project-management software — no app, no accounts, no sync. Plain markdown and a habit.
- Not a note-taking method — it deliberately manages *state*, not knowledge.
- Not tied to any AI vendor — the files are the system; any assistant that can read them can run it.

It was built and refined in daily use running a real company's projects with AI assistants — the conventions here are the survivors, not the theory.
