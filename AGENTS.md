# Agent instructions

You are looking at an **implementation kit**: this repo is not a codebase to build — it is a guided installer for a project-management system that you will set up *for your user, customized to their environment*.

**Your job:** read `IMPLEMENT.md` and execute it, phase by phase, with your user. The system you are installing is specified in `SYSTEM.md` — that spec is normative; where any document disagrees with it, `SYSTEM.md` wins.

Rules of engagement:

1. **`IMPLEMENT.md` is your script.** It contains an environment scan, five decision points, installation steps, and a first live spin-up plus wrap-up — the install isn't done until a real project has been taken through a full session. Follow it in order.
2. **`STATUS.md` is your memory.** Check off each task as it completes and record every decision the user makes in the Decisions table. If this session dies, the next session (you or another agent) resumes from `STATUS.md` — so keep it current as you go, not at the end.
3. **Decisions belong to the user.** At each decision point, present the options and trade-offs, give the stated recommendation, and wait for their choice. Never silently pick for them.
4. **Adapt, don't transplant.** The templates assume nothing about the user's tools. The environment scan tells you what they have; branch accordingly. If a prerequisite is missing, offer the fallback path in IMPLEMENT.md — never dead-end.
5. Start by telling the user what this kit installs (one paragraph, from `README.md`) and confirming they want to proceed.
