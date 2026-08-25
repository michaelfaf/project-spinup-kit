<!-- Template. CURRENT STATE ONLY — the test: "Would an agent loading current state need this paragraph?" No → History/ at next retire. Hard cap ~50 lines: over cap at retire → sweep oldest completed items and narrative to History/LOG.md. All dates from the system clock, never assumed. Delete unused sections; depth scales with the project. -->

# Status — <PROJECT NAME>

State: active
<!-- active | parked | waiting-external | done (SYSTEM.md §7). A push session writes
"Session open: <timestamp>" below this line at entry and clears it at retire. -->

## Current position

<1–3 dated lines. What just happened, what's live right now. Rewritten at every retire.>

## Board

> Legend: `- [ ]` task · 🔴 blocks completion · ⚖️ needs the user's decision · ❓ verify · owner in parens (user/AI/either)

<!-- When a push's "Done means" passes: tick it, move it to Completed, open the next push. Never roll a satisfied push forward with new tasks — rewrite the goal as a new push instead. -->

### P1 — <push name>
**Done means:** <one binary line, written before execution starts>
- [ ] <task> (AI)

### P2 — <push name>
**Done means:** <...>
- [ ] <task> (user)

**✅ Completed (current push only — older items sweep to History/LOG):**
- [x] <task> — <date>

## Waiting on

- <who> · <what> · since <date> · **chase on <date>**

## Standing / recurring

- <item> · <cadence> · last done <date> · next due <date>

## Inbox

<!-- Dated one-liners dropped here by sessions working OTHER projects. Triage at entry: promote to board, blocker, or discard. -->
- <date> from <project>: <one line>

## Blockers / open decisions

- ⚖️ <decision waiting on the user>
- 🔴 <blocker> — or "none"
