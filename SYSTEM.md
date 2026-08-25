# SYSTEM — the Project Spin-Up System, specified

> **This is the normative spec.** It states the rules of the system — compactly, with no
> persuasion and no installation procedure. Where any other document in this kit
> disagrees with this one, this one wins. *Explanation* lives in `OVERVIEW.md`;
> *installation* lives in `IMPLEMENT.md`; concrete file shapes live in `templates/`.
> Key words: **must** = required for a conformant install; **per configuration** =
> governed by a decision axis in §8.

## 1. Invariants

Everything else in this document derives from five rules:

- **I1 — Memory lives in files.** A project's memory lives in files in the project's own
  folder — never only in a chat, and never only in a platform memory feature. Prompts
  live on disk, not in chat: the chat can die; the work can't.
- **I2 — Three jobs, three homes.** Every piece of project information has exactly one
  job — *identity*, *current state*, or *history* — and each job has exactly one home
  (§2). Information in the wrong home is a defect.
- **I3 — One fact, one file.** One fact lives in exactly one file; every other mention
  points at it. A duplicated fact is a defect even while the copies still agree.
- **I4 — Sessions enter and retire.** Every working session enters by reading the
  project's state and retires by writing the state back (§4). A session that skips
  retirement has stranded its work; retiring properly is the non-negotiable step.
- **I5 — Done is declared before the work.** Every push carries a binary "Done means:"
  line written before execution starts, and is verified against actual output — never
  against memory (§3, §5).

## 2. The file contract

A project is a folder. Its files, their jobs, and their boundaries:

| File | Job | Updated when | Must never contain |
|---|---|---|---|
| `README.md` | Identity: what/why/how it works, standing rules, locked decisions | Scope or a decision changes | Current state |
| `STATUS.md` | Current state only: dated position line(s), the board, blockers / open decisions | Every session retire | History, rationale, or anything failing the STATUS test |
| `History/LOG.md` | Rolling session log, newest first, one entry per session | Every session retire (append) | — |
| `History/LESSONS.md` *(per configuration, DP-5 = B or C)* | Append-only lessons: what happened / why / rule going forward | When a lesson lands | Edits to past entries |
| `NEXT-CHAT.md` *(per configuration, DP-4 = A)* | The opening prompt for the next session: where we are, decided, do next, open threads | Written/rewritten at retire while a multi-session push is live; **deleted when the push ends** | Narrative |
| Decision briefs *(per configuration, DP-5 = C; home directory not yet standardized)* | One numbered brief per researched decision, ending in a *lean, never a ruling* — the human rules | When a decision needs research | Rulings |

**The STATUS test.** Applied to `STATUS.md` constantly: *"Would an agent loading current
state need this paragraph?"* No → it moves to `History/` at the next retire. A STATUS
file that scrolls has failed.

**Minimum conformant shape.** `README.md` and `STATUS.md` must exist from spin-up;
`History/` must exist and `History/LOG.md` must exist from the first retired session.
Everything else is per configuration (§8). Depth scales: files grow only when the work
earns it.

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
- Blockers and decisions waiting on the user are marked on the board and listed under
  Blockers / open decisions.

## 4. The session protocol

Every working session, in order:

1. **Enter** — read the project's `README.md` + `STATUS.md` (and the parent project's,
   if nested); read `NEXT-CHAT.md` if present.
2. **Plan** — state what this session will do; give every new push a valid
   "Done means:" line (§3).
3. **Plan Check** — per configuration (DP-3), run the gate in §5 before executing.
4. **Execute** — work the board.
5. **Ship Check** — per configuration (DP-3), run the gate in §5 after executing.
6. **Retire** — in order: (a) update STATUS's dated position line; (b) tick the board
   and sweep anything failing the STATUS test to `History/`; (c) append the session to
   `History/LOG.md`; (d) write the handoff per configuration (DP-4); (e) per
   configuration (DP-5 = B or C): if something taught a lesson, append it to
   `History/LESSONS.md` and propose any promotion (§6).

## 5. Quality gates

Gates are run by fresh eyes. Which kind is per configuration (DP-3): under **A**, each
gate runs in a fresh-context subagent that never saw the working conversation; under
**B**, the AI reviews itself against these checklists verbatim; under **C**, gates are
skipped.

**Plan Check** (after planning, before execution — premise first, correctness last):
1. Is the premise right — is there a simpler or higher-level way to get the outcome?
2. Does every push have a binary "Done means:" line?
3. Does any step depend on a tool or fact not verified?
4. What's missing that will surface mid-execution?

**Ship Check** (after execution):
1. Does each "Done means:" line pass, checked against the actual output — not memory?
2. Will any of this have to be redone? If so, flag it now.

## 6. The lessons loop

- A **lesson** is 2–4 lines appended to `History/LESSONS.md`: what happened, why, the
  rule going forward.
- A lesson that should change future behavior is **promoted**: the AI proposes it, the
  user signs off, and it lands in the project README's standing rules — where every
  future session will obey it. Promotion never happens without the user's sign-off.

## 7. Roles

- **The human rules.** Promotions, the decisions briefs lean toward, and every §8
  configuration choice belong to the user. Briefs and options end in a lean, never a
  ruling.
- **The AI does the mechanics.** Reading state on entry, keeping STATUS true, logging,
  handoffs, running the gates, proposing lessons and promotions.

## 8. Configuration axes

Five decisions fix an install's shape. Legal values only — the options' trade-offs and
recommendations live in `IMPLEMENT.md` Phase 1:

| Axis | Decision | Legal values |
|---|---|---|
| DP-1 | Where projects live | **A** existing workspace · **B** dedicated folder · **C** chat-managed — plus sub-decision: git yes/no |
| DP-2 | Default depth per project | **A** light · **B** full from day one |
| DP-3 | Quality gates | **A** subagent gates · **B** self-review gates · **C** none |
| DP-4 | Session handoff | **A** `NEXT-CHAT.md` file · **B** platform memory · **C** manual paste |
| DP-5 | History depth | **A** LOG only · **B** LOG + LESSONS · **C** LOG + LESSONS + decision briefs |

## 9. Verification: the cold-read test

A project conforms when a **fresh** session — one that has seen nothing but the folder —
can open it cold and state exactly where things stand and what's next, from `README.md`
+ `STATUS.md` alone. This is also the acceptance test for an install (`IMPLEMENT.md`
Phase 3) and the standing health check for any project: if the cold read fails, the
files have drifted from this spec.

## 10. Document ownership (this kit's repo)

*This section governs the kit repo only — drop it if this spec is lifted into a
workspace.*

| Document | Owns | Defers to SYSTEM.md on |
|---|---|---|
| `SYSTEM.md` | The rules: invariants, contracts, protocol, gates, axis names/values | — |
| `README.md` | The pitch; how to use the kit | All system rules |
| `OVERVIEW.md` | Explanation: concepts, rationale, what a week looks like | All system rules |
| `IMPLEMENT.md` | Install procedure; DP option prose, trade-offs, recommendations | Rule content it installs |
| `AGENTS.md` / `CLAUDE.md` | Agent entry point; rules of engagement for the installer | All system rules |
| `templates/` | Concrete file shapes | The jobs and boundaries in §2–§3 |
| `STATUS.md` (kit root) | This kit's install progress | — |
