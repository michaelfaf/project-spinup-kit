# templates/

The files the implementing AI copies (and adapts) into the user's workspace as `(PROJECT TEMPLATE)/`. Which ones get installed depends on DP-4 and DP-5 in `IMPLEMENT.md`; DP-2 decides how much of the set each new project starts with.

| Template | In `(PROJECT TEMPLATE)/` when | A project gets its copy when |
|---|---|---|
| `PROJECT-README.md` → `README.md` | always | spin-up |
| `PROJECT-STATUS.md` → `STATUS.md` | always | spin-up |
| `LOG.md` → `History/LOG.md` | always | DP-2 full: spin-up · light: first retire |
| `LESSONS.md` → `History/LESSONS.md` | DP-5 = B or C | DP-2 full: spin-up · light: first lesson |
| `NEXT-CHAT.md` | DP-4 = A | created at retire while a multi-session push is live — never pre-created |
| `DECISION-BRIEF.md` | DP-5 = C | one numbered copy per researched decision — never pre-created |
