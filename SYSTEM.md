# SYSTEM — the Project Spin-Up System, specified

> **This is the normative spec.** It states the rules of the system — compactly, with no
> persuasion and no installation procedure. Where any other document in this kit
> disagrees with this one, this one wins. *Explanation* lives in `OVERVIEW.md`;
> *installation* lives in `IMPLEMENT.md`; concrete file shapes live in `templates/`.
> Key words: **must** = required for a conformant install; **per configuration** =
> governed by a decision axis in §10; **default** = the standard behavior unless an
> install deliberately varies it.

## 1. Invariants

Everything else in this document derives from six rules:

- **I1 — Memory lives in files.** A project's memory lives in files in the project's own
  folder — never only in a chat, and never only in a platform memory feature. Prompts
  live on disk, not in chat: the chat can die; the work can't.
- **I2 — Files are authoritative.** If platform memory conflicts with README or STATUS,
  the file wins and the memory is corrected.
- **I3 — Three jobs, three homes.** Every piece of project information has exactly one
  job — *identity*, *current state*, or *history* — and each job has exactly one home
  (§2). Information in the wrong home is a defect.
- **I4 — One fact, one file.** One fact lives in exactly one file; every other mention
  points at it. A duplicated fact is a defect even while the copies still agree.
- **I5 — Sessions enter and retire.** Every working session enters by reading the
  project's state and retires by writing the state back (§4). A session that skips
  retirement has stranded its work; retiring properly is the non-negotiable step.
- **I6 — Done is declared before the work.** Every push carries a binary "Done means:"
  line written before execution starts, and is verified against actual output — never
  against memory (§3, §5).

## 2. The file contract

A project is a folder. Projects are **flat**: related work is a sibling project, linked
by a pointer line in README — projects do not nest.

### Project files

| File | Job | Updated when | Must never contain |
|---|---|---|---|
| `README.md` | Identity: what/why/how it works, standing rules, locked decisions | Scope or a decision changes | Current state |
| `STATUS.md` | Current state only: a `State:` line, dated position line(s), the board, waiting-on, standing/recurring work, inbox, blockers | Every retire (and marker/inbox writes, §4) | History, rationale, or anything failing the STATUS test |
| `History/LOG.md` | Session log, chronological, one entry appended per session | Every retire (append only — prior entries are never rewritten) | Edits to past entries |
| `History/LESSONS.md` | Append-only lessons: what happened / why / rule going forward | When a lesson lands | Edits to past entries |
| `History/Decisions/NNN-topic.md` | One numbered brief per researched decision, ending in a *lean, never a ruling* — the human rules. A historical record, tucked away: when ruled, the ruling gets a row in README's Locked decisions pointing at the brief | When a decision needs research | Rulings inside the brief body |
| `NEXT-CHAT.md` | Handoff: the 2–4 lines the next session can't get from STATUS — "start with…", "watch out for…" | Written/rewritten at retire while a multi-session push is live; **deleted when the push ends** | Anything that restates STATUS, the board, LOG, or blockers |

**`STATUS.md` rules.**
- First line after the title: `State: active | parked | waiting-external | done` (§7).
- **The STATUS test**, applied constantly: *"Would an agent loading current state need
  this paragraph?"* No → it moves to `History/` at the next retire.
- **Hard cap: ~50 lines / one screen.** Over cap at retire → sweep the oldest completed
  items and any narrative to `History/LOG.md` before writing. The cap fires the sweep;
  the STATUS test decides what gets swept.
- Every date in STATUS (and LOG) comes from the **system clock** (or from asking the
  user) — never from the model's assumption.

**`NEXT-CHAT.md` precedence.** If NEXT-CHAT disagrees with STATUS, STATUS wins — delete
NEXT-CHAT and re-derive. (Abnormal session ends leave orphans; never trust one over
STATUS.)

**`History/LOG.md` rotation.** Entries append chronologically at the end; read the last
few entries for recent history. At ~300 lines, rotate to `History/LOG-<YYYY>.md` and
start fresh. Never rewrite or summarize prior entries.

**Minimum conformant shape.** `README.md` and `STATUS.md` must exist from spin-up;
`History/` must exist and `History/LOG.md` from the first retired session. LESSONS,
briefs, and NEXT-CHAT appear when the work first calls for them (defaults, §10). Depth
scales: files grow only when the work earns it.

### Workspace files (one level above the projects)

| File | Job | Updated when |
|---|---|---|
| `PORTFOLIO.md` | One row per project: name · state · last touched · next action · waiting-on. **STATUS wins on disagreement** — a stale row is rewritten, never trusted | Every retire; read by the weekly review (§8) |
| `LESSONS.md` | General lessons — true everywhere, not just in one project (§6) | When a general lesson is promoted |
| `SYSTEM.md` | This spec, minus §12 — installed so the rules survive the kit and any AI can read them | When the kit's spec is updated |

**Filenames are conventions; jobs are the system.** An install may keep a user's
existing filenames, provided each job above maps onto exactly one file.

## 3. The board: pushes, tasks, "Done means:"

- The board lives in `STATUS.md`. A **push** is a unit of outcome; a **task** is a step
  inside one. Each task names an owner (user / AI / either).
- A **"Done means:" line is valid** when it is one line, binary (pass/fail with no
  judgment call), testable against actual output, and written before execution starts.
- **Close, don't roll.** When a push's "Done means" passes: tick it, move it to
  Completed, open the next push. Never roll a satisfied push forward with new tasks —
  rewrite the goal as a new push. Completed items older than the current push sweep to
  `History/LOG.md`.
- **Waiting on** — third-party blocks get a dated four-field line: who · what · since ·
  **chase on** (the date to follow up). First thing the weekly review reads.
- **Standing / recurring** — operational work with no terminal done state (invoicing,
  follow-ups) gets its own section: item · cadence · last done · next due. No "Done
  means:", never swept to Completed; the weekly review flags overdue items.
- **Inbox** — a session working project A that finds something affecting project B
  writes one dated line into B's STATUS Inbox before retiring. The receiving session
  triages its Inbox at entry: promote to board, blocker, or discard.
- Blockers and decisions waiting on the user are marked on the board and listed under
  Blockers / open decisions.

## 4. The session protocol

There are two session classes. Naming the cheap path is what protects the full one.

**Touch** — one task, no decision changed, no new push. No plan, no gates; exit is a
board tick (or nothing). **Upgrade trigger:** the moment a touch grows past one task,
changes a decision, or reveals a blocker, stop and convert to a push session.

**Push session** — everything else. In order:

1. **Enter** — read the project's `README.md` + `STATUS.md`; read `NEXT-CHAT.md` if
   present (STATUS wins on conflict, §2). Then three checks:
   (a) *concurrency* — if STATUS carries a `Session open:` marker less than a few hours
   old, another session may be live: say so and ask before proceeding; otherwise write
   `Session open: <timestamp>` into STATUS;
   (b) *staleness* — compare STATUS's last date against the newest file in the folder;
   if other files are newer, say so and reconcile before planning;
   (c) *inbox* — triage any Inbox lines (§3).
2. **Plan** — state what this session will do; give every new push a valid
   "Done means:" line (§3).
3. **Plan Check** — per configuration (DP-2), run the gate in §5 before executing.
4. **Execute** — work the board.
5. **Ship Check** — per configuration (DP-2), run the gate in §5 after executing.
6. **Retire** — the close-out checklist, every item binary, in order:
   1. Position line rewritten, dated from the system clock.
   2. Board matches reality: ticks current, completed swept, STATUS within its cap.
   3. Blockers and waiting-ons current, chase dates set.
   4. Any decision made this session → README's Locked decisions.
   5. Any scope change → README.
   6. Anything learned → `History/LESSONS.md`, with promotion proposed (§6).
   7. Session appended to `History/LOG.md`, including gate verdicts (§5).
   8. Handoff written — or NEXT-CHAT deleted if the push ended.
   9. `PORTFOLIO.md` row updated.
   10. `Session open:` marker cleared; if git is available (default, §10), commit:
       "retire: <project> <date>".

## 5. Quality gates

Gates are run by fresh eyes. Which kind is per configuration (DP-2): under **A**, each
gate runs in a fresh-context subagent that never saw the working conversation; under
**B**, the AI reviews itself against these checklists verbatim; under **C**, gates are
skipped. Installed rules state it capability-conditionally — "a subagent if this
platform has them, otherwise a clean self-review" — so the same install works when a
different AI opens the folder.

**Gate input packet** (and nothing from the working conversation): the project README +
STATUS, the plan, and its "Done means:" lines.

**Plan Check** (after planning, before execution — premise first, correctness last):
1. Is the premise right — is there a simpler or higher-level way to get the outcome?
2. Does every push have a binary "Done means:" line?
3. Does any step depend on a tool or fact not verified?
4. What's missing that will surface mid-execution?

**Ship Check** (after execution):
1. Does each "Done means:" line pass — **with quoted evidence per line** (a path, an
   output, a snippet), never a bare assertion?
2. Will any of this have to be redone? If so, flag it now.

**Verdicts and the failure path.** Every gate emits one recorded verdict into the LOG
entry: `pass | pass-with-changes | fail(reason) | skipped(reason)` — a skipped gate is
visible data, not a silent nothing. On `fail`: revise once and re-check; a second
`fail` stops the session and escalates to the user.

## 6. The lessons loop

- A **lesson** is 2–4 lines appended to `History/LESSONS.md`: what happened, why, the
  rule going forward.
- A lesson that should change future behavior is **promoted** — and promotion asks one
  routing question first: *"true only here, or everywhere?"*
  - **Only here** → the project README's standing rules.
  - **Everywhere** → the workspace `LESSONS.md`; standing ones are copied into the
    user's AI instructions file (capped — prune at the weekly review).
- Promotion never happens without the user's sign-off. The weekly review (§8) surfaces
  the pending-promotion queue, because sign-offs otherwise never happen.

## 7. Project lifecycle

Every STATUS carries `State: active | parked | waiting-external | done`.

**Archive** (when a project is done): final LOG entry · harvest lessons upward (§6) ·
set `State: done` · move the folder to the archive location (per configuration, DP-3;
default `Archive/<YYYY>/` inside the projects root) · remove its PORTFOLIO row (the
archive folder is the record).

## 8. Portfolio and the weekly review

`PORTFOLIO.md` (§2) answers "what's live, what's stalled, who am I waiting on" without
opening folders. The **weekly review** — 10 minutes, run by the AI on the chosen day
(per configuration, DP-4) — reads PORTFOLIO and every active project and reports six
things:

1. Projects untouched longer than 14 days.
2. STATUS position lines older than their folder's newest file.
3. Blockers older than 14 days.
4. Waiting-ons past their chase date.
5. `NEXT-CHAT.md` files with no live push (orphans — delete per §2).
6. Lessons awaiting promotion sign-off (§6).

The review is a report plus proposed actions; the user rules on anything non-mechanical.

## 9. Roles

- **The human rules.** Promotions, the decisions briefs lean toward, rulings from the
  weekly review, and every §10 configuration choice belong to the user. Briefs and
  options end in a lean, never a ruling.
- **The AI does the mechanics.** Reading state on entry, keeping STATUS true, logging,
  handoffs, running the gates, the weekly review, proposing lessons and promotions.

## 10. Configuration axes and defaults

Five decisions fix an install's shape. Legal values only — trade-offs and
recommendations live in `IMPLEMENT.md` Phase 1:

| Axis | Decision | Legal values |
|---|---|---|
| DP-1 | Where projects live | **A** existing workspace · **B** dedicated folder · **C** chat-managed |
| DP-2 | Quality gates | **A** subagent gates · **B** self-review gates · **C** none |
| DP-3 | Archive location | a folder path (default `Archive/<YYYY>/` inside the projects root) |
| DP-4 | Weekly review day | a weekday (default: end of the working week) |
| DP-5 | General-lesson destination | **A** workspace `LESSONS.md`, promoted into the instructions file (recommended) · **B** instructions file directly |

**Defaults — standard behavior, not decisions:**
- Depth: light. New projects start as README + a short STATUS + empty `History/`;
  files grow when earned.
- History: LOG + LESSONS; decision briefs when a decision first needs real research.
- Handoff: `NEXT-CHAT.md` on file-capable platforms; manual STATUS paste under DP-1 C.
- Git: on, when available — it is the system's undo. Retire commits per §4. Skip only
  where git genuinely doesn't exist.

## 11. Verification: the cold-read test

A project conforms when a **genuinely fresh** reader — a fresh-context subagent or a
brand-new chat given only the text of `README.md` + `STATUS.md`, having seen nothing
else — answers all four correctly: what the project is · current position · next
action · blockers. 4/4 is the pass bar. Self-grading by the session that wrote the
files does not count. This is the acceptance test for an install (`IMPLEMENT.md`
Phase 3) and the standing health check for any project.

## 12. Document ownership (this kit's repo)

*This section governs the kit repo only — it is dropped from the copy installed into a
workspace (§2).*

| Document | Owns | Defers to SYSTEM.md on |
|---|---|---|
| `SYSTEM.md` | The rules: invariants, contracts, protocol, gates, axis names/values | — |
| `README.md` | The pitch; how to use the kit | All system rules |
| `OVERVIEW.md` | Explanation: concepts, rationale, what a week looks like | All system rules |
| `IMPLEMENT.md` | Install procedure; DP option prose, trade-offs, recommendations | Rule content it installs |
| `AGENTS.md` / `CLAUDE.md` | Agent entry point; rules of engagement for the installer | All system rules |
| `templates/` | Concrete file shapes | The jobs and boundaries in §2–§3 |
| `INSTALL-STATUS.md` | This kit's install progress | — |
