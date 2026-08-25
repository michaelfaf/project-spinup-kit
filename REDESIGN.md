# REDESIGN — proposals for the next version

> **Working document — do not merge to main; delete before release.** This is the
> redesign review Mike asked for: recommendations from two independent fresh-context
> reviews (a systems-design critique and a docs-consistency sweep) plus the
> orchestrator's synthesis. Nothing here is implemented; every item waits on Mike.

**How the tiers work**

- **Tier 1 — say yes and move on.** The kit already states the intended behavior
  somewhere; the change just makes the docs agree or fills a documented gap. No new
  design decisions.
- **Tier 2 — real changes, your approval required.** Each changes a rule, a template,
  or how the system works. Recommended, with cost/risk stated.
- **Tier 3 — genuinely unsure.** Open design questions where I won't pretend to have
  the answer; options given, no ruling.

---

## Tier 1 — obvious fixes

### R1. Make the docs agree with themselves (consistency bundle)
The sweep found the same rule stated differently across files. Fixes, none changing
what the system *means*:
- The session protocol appears as 6 steps (OVERVIEW), 5 steps (README, IMPLEMENT
  Phase 3), and 3 steps (IMPLEMENT intro). Align all to SYSTEM.md §4's six steps.
- Only the first gate is marked "(optional)" in OVERVIEW; DP-3 governs both. Mark both.
- NEXT-CHAT.md's write condition ("only while a multi-session push is live") appears
  only in the template; OVERVIEW/IMPLEMENT imply every retire. Add the qualifier.
- DP-2 "Full: all files pre-created" contradicts templates/README.md ("NEXT-CHAT.md —
  not pre-created"). Add the exception to DP-2.
- The STATUS test says "agent" in two files and "AI" in the template. Pick "agent".
- IMPLEMENT's intro file table presents LESSONS as unconditional and omits NEXT-CHAT;
  both are configuration-dependent. Mark them.
- IMPLEMENT Phase 2 says files are gated on "DP-2/DP-5"; the actual gating (in
  templates/README.md) is DP-4/DP-5. Correct it, and have Phase 2 point at
  templates/README.md explicitly — today that mapping table (including the
  PROJECT-README.md → README.md rename) is orphaned; a literal installer AI is never
  told to open it and could install wrongly-named files.
- Define "push" on first use in IMPLEMENT (it's only defined in OVERVIEW); gloss "the
  source system" (undefined antecedent); README calls `History` a *file*; AGENTS.md's
  summary omits Phases 3–4.
- Deduplicate: the one-file rule, the STATUS test, the promotion rule, the briefs
  "lean, never a ruling" phrase, and NEXT-CHAT's lifecycle are each stated in full in
  2–3 places. Now that SYSTEM.md is the owner, collapse the copies to pointers.

*Cost/risk: none — wording only. This is the kit obeying its own headline rule.*

### R2. Ship the missing Decision Brief template
DP-5 option C sells a "numbered brief" tier; templates/ contains no brief template, no
home directory, no link to README's Locked decisions table. A user choosing C has
nothing to copy. Add `templates/DECISION-BRIEF.md` (context · options · evidence ·
lean), a home (`Decisions/NNN-topic.md` or `History/`), and the closing rule: a ruled
brief writes one row into README's Locked decisions and the brief becomes history.
Same bundle: define or delete "superseded docs" (promised in PROJECT-README's Pointers,
exists nowhere).

*Cost/risk: none — fills a documented promise.*

### R3. Files beat platform memory, explicitly
Memory features now run concurrently with files on every major platform and will
eventually contradict STATUS invisibly (e.g. remembering a since-reversed decision).
One installed rule: "Files are authoritative. If platform memory conflicts with
README/STATUS, the file wins and the memory is corrected." One line in SYSTEM.md §1
and the Phase 2 instructions block.

*Cost/risk: none — it's already the system's implicit position (I1); this states it.*

---

## Tier 2 — real changes, recommended, need your approval

### R4. A close-out checklist — the highest-value small change
Today the *optional* gates have the only checklists in the kit, while the
*non-negotiable* step (retire) is a prose sentence executed from memory — exactly
backwards. Make retire a literal 9-item binary checklist (position line has today's
real date · board matches reality · blockers current · decisions made this session →
README Locked decisions · scope changes → README · lessons → LESSONS · LOG appended ·
handoff written-or-deleted · portfolio row updated), shipped in the template and
SYSTEM.md §4.

*Cost: one minute per session. Risk: essentially none. If you approve only one Tier 2
item, make it this one.*

### R5. Two session classes: "touch" vs "push session"
Daily reality is dominated by two-minute asks. Forcing plan → gate → execute → gate →
retire onto them means the protocol gets skipped, and skipping becomes the habit that
then erodes real sessions. Sanction the cheap path: a **touch** (one task, no decision
changed, no new push) needs no plan, no gates, exit = a board tick; with an explicit
upgrade trigger ("grows past one task, changes a decision, or hits a blocker → convert
to a push session").

*Cost: a definition and a judgment call per session. Risk: the touch loophole gets
stretched — the upgrade trigger is the guard.*

### R6. A portfolio layer: `PORTFOLIO.md` + a weekly review
The system's audience runs 2+ projects, but every mechanic is scoped to one folder —
"what's live, what's stalled, what's waiting on someone" has no home, so the operator's
memory becomes the index again (the exact failure the system exists to fix, one level
up). Two coupled pieces:
- **PORTFOLIO.md** at the workspace root: one row per project (name · state · last
  touched · next action · waiting-on), updated at every close-out. STATUS wins on
  disagreement — a stale row is rewritten, never trusted.
- **A weekly review ritual** the AI runs against it: projects untouched > N days ·
  position lines older than the folder's newest file · blockers > 14 days · waiting-ons
  past chase date · orphaned NEXT-CHATs · lessons awaiting promotion. Today the system
  is purely event-driven: a project nobody opens emits no signal, so rot is invisible
  by construction.

*Cost: one file + one 10-minute weekly session. Risk: the row is cached state (a
deliberate, precedence-ruled exception to one-fact-one-file).*

### R7. Project lifecycle: a `State:` line and an Archive step
Projects never end in this system — no done state, no archive, no closing ritual. A
year in, `Projects/` is 40 folders, 9 live, and finding out which means opening them.
Add `State: active | parked | waiting-external | done` at the top of STATUS, and an
Archive step: final LOG entry · harvest lessons upward (R8) · set done · move to
`Projects/Archive/YYYY/`.

*Cost: small. Risk: low; needs one new decision (archive location — Tier 3, Q4).*

### R8. Lessons that cross projects
Promotion today runs LESSONS → *that project's* README. But most real lessons are
general ("never send a quote without X", "always confirm the date from the shell") and
stay trapped where they happened — so only one project gets smarter, and projects 2–4
re-learn it. Add the routing question to promotion: *"true only here, or everywhere?"*
Project-specific → project README; general → workspace `LESSONS.md` / your instructions
file. Review the pending-promotion queue in the weekly review (R6), since sign-off
otherwise never happens.

*Cost: one question per promotion. Risk: instructions-file bloat — cap it and prune at
review.*

### R9. Harden the gates: verdicts, evidence, a failure path
The gates currently emit nothing, record nothing, and have no failure path — a
"looks good" under time pressure is indistinguishable from a real check, and a skipped
gate leaves no trace. Three changes: (1) every gate emits a recorded verdict in the LOG
entry — `pass | pass-with-changes | fail(reason) | skipped(reason)` — making *skipped*
visible data; (2) fail → revise once → re-check; second fail → stop and escalate to
you; (3) the Ship Check must quote evidence per "Done means" line (a path, an output, a
snippet), not assert it. Plus two wording fixes: the installed gate rule becomes
capability-conditional ("subagent if the platform has them, else clean self-review") so
a different AI on Wednesday isn't reading broken instructions, and the gate's input
packet is specified (README + STATUS + plan + Done-means lines, nothing from the
working conversation) so a fresh subagent doesn't return generic advice.

*Cost: a few lines per session. Risk: ceremony — the verdict token is one line, kept
deliberately cheap.*

### R10. Trust but verify STATUS: real dates + a staleness check
The whole system rests on STATUS being current, and nothing checks it. AIs routinely
write wrong/carried-forward dates, and a stale-but-confident STATUS is worse than none
because it's *trusted*. Two rules: (1) every date the AI writes comes from the system
clock (or asking you), never assumption; (2) on entry, compare STATUS's date against
the folder's newest file — if other files are newer, say so and reconcile before
planning. Companion: replace the un-enforced "a STATUS that scrolls has failed" with a
number and a trigger — hard cap ~50 lines; over cap at close-out → sweep to History.

*Cost: seconds. Risk: none worth naming.*

### R11. Fix NEXT-CHAT's drift problem
NEXT-CHAT's four sections all restate facts that have homes (position, LOG, board,
blockers) — the system's own one-file rule broken by its own template — and its
delete-when-done lifecycle guarantees orphans after abnormal session ends, which the
*next* session then reads first and trusts over STATUS. Strip it to the non-duplicable
2–4 lines ("Start with: … Watch out for: …"), add the precedence rule ("disagrees with
STATUS → STATUS wins, delete and re-derive"), put deletion on the close-out checklist
and orphan-detection in the weekly review.

*Cost: template rewrite. Risk: less context handed to the next session — mitigated
because STATUS (kept honest by R4/R10) was always the real handoff.*

### R12. Installer robustness: fresh cold-read + idempotent re-run
(a) Phase 3's acceptance test is self-graded: the session that just wrote README+STATUS
"verifies" them with the whole project in context — it passes every time. Make it
genuinely fresh: a subagent (or new chat) given only the two files' text, pass = 4/4 on
what-it-is / position / next action / blockers. (b) Phase 0 never asks "is this already
installed?" — re-running after a dead session or on a second machine duplicates the
instructions block and re-asks settled decisions. Add the check: existing template
folder or convention block found → upgrade mode, diff and merge, pre-fill Decisions.
Companion small fixes: LOG declared append-only/never-rewritten with yearly rotation
(newest-first + rewrite-on-every-entry is where AIs truncate history), git promoted
from guilt-free-skip to default-when-available with a close-out commit (it's the
system's only undo), and a `## Waiting on` STATUS section (who · what · since · chase
on) for third-party blocks, plus a `## Standing / recurring` section (item · cadence ·
last done · next due) so operational work stops needing fake "Done means" lines.

*Cost: moderate — this is the biggest IMPLEMENT/template edit in Tier 2. Risk: low;
each piece is independently droppable.*

---

## Tier 3 — open questions, no ruling from me

### Q1. Should SYSTEM.md ship into the user's workspace?
Phase 4 invites the user to delete the kit — but the gate checklists, STATUS test, and
promotion loop then exist nowhere in the installed system except a three-bullet
instructions block, and a different AI opening the workspace has no manual (which
quietly breaks the vendor-neutrality promise). SYSTEM.md was written "liftable" so
Phase 2 could install a copy (as workspace `SYSTEM.md` or `PROTOCOL.md`) that the
instructions block points at. **For:** the spec survives; drift stops. **Against:** one
more root file to keep in sync with the kit. My lean is yes, but it changes what "the
install" is — your call.

### Q2. Collapse the five decision points to the ones that matter?
The critique argues DP-2 and DP-5 are near-duplicates (both "how much scaffolding",
both recommend light), DP-4's options B/C are recommended-against or forced by DP-1 —
so ~2.5 of 5 decisions are pseudo-decisions, while genuinely varying questions (archive
location, review cadence, general-lesson destination) are never asked. Collapsing
changes the installer's spine and STATUS.md's Decisions table, and "five decisions" is
part of the kit's pitch. Real redesign surgery — needs your read on whether install
simplicity or configurability is the product.

### Q3. Nested projects: specify or cut?
Nesting is referenced three times ("and the parent's, if nested") and specified zero
times — parent/child state mirroring, board ownership, rule inheritance, depth are all
undefined, so every install resolves them differently. Options: (a) specify one rule
set (parent STATUS carries one line per child; children own their state; rules inherit
by reference; max one level), or (b) cut nesting from v1 and make everything siblings.
Both defensible; (b) is simpler and honest.

### Q4. Smaller unresolved calls
- **Rename "retire" → "close out"?** "Retire" naturally reads as retiring the
  *project* (which R7 introduces for real). Clearer, but churns vocabulary you may
  already have muscle memory for.
- **Cross-project Inbox** (a session that finds something affecting project B writes
  one dated line into B's STATUS): real problem, unproven mechanism, more ceremony.
- **Session-open marker** (timestamp in STATUS at entry to catch two concurrent chats
  clobbering each other): cheap, but adds a step to every entry for a sometimes
  problem.
- **Archive location + review day** (needed if R6/R7 land): where and when.
- **Rename the kit's root STATUS.md** (e.g. `INSTALL-STATUS.md`) so it stops
  name-colliding with the per-project STATUS.md it teaches — or reshape it to follow
  its own template as dogfooding.

---

## Also asked along the way

- **"The user interface stuff"** — you referenced something I built for the UI before
  this session; this remote session starts cold from the repo and nothing UI-related
  exists here. If SYSTEM.md should connect to that artifact, I need the pointer.
- **Scope of "the redesign"** — docs, mechanics, or both? This document covers both so
  you can scope it when we walk through.
