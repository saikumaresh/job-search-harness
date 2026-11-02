# Job-search harness for Claude Code

A better model will not fix your job search. A better system around the model will.

This is a reference build of harness engineering, applied to a problem everyone understands: finding the right role. Claude finds roles, scores every posting the same way every time with quoted evidence, gets a second Claude to check its work, remembers what it has already seen, learns only from corrections you approve, and runs on a schedule. You make every decision. Nothing is sent without your yes.

Built by [Sai Kumaresh](https://www.linkedin.com/in/sai-kumaresh/). Write-up: {{MEDIUM_LINK}}

## The six layers

| Layer | What it is here | Files |
|---|---|---|
| 1. Context | Your search brief, a 1/3/5 rubric where every score needs a quote from the JD, an example of strong evidence with a "do not copy" warning, and a ledger of what you can truthfully claim | `CLAUDE.md`, `context/` |
| 2. Tools and limits | What Claude can read and touch, and a hard "never" list (no applying, no sending, no accounts, no passwords) | `CLAUDE.md` |
| 3. Output | Four skills that run the same steps and write the same report shape every time | `.claude/skills/`, `context/templates/` |
| 4. Verification | A separate, read-only verifier agent that greps every quote against the JD and rechecks the maths. It is allowed to say CONFIRMED | `.claude/agents/verifier.md` |
| 5. Memory | A seen-postings log so nothing is screened twice, a run log, a tracker, and a calibration file that only changes when you approve | `memory/` |
| 6. Automation | Scheduled tasks in the Claude desktop app | `SCHEDULE.md` |

## Commands

- `/find-jobs` finds, scores, verifies and reports. Discovery only.
- `/prep-application <company> <role>` builds a tailored one-page resume, cover note and screening answers, verified, then stops for your yes.
- `/follow-ups` reads replies, updates stages and drafts follow-ups. Drafts only.
- `/calibrate <date> <feedback>` turns your disagreement into one general rule, shows what it would change, and saves it only after you approve. Claude can never run it on its own.

## Set it up in about 30 minutes

1. Install the Claude desktop app and open **Claude Code** on this folder.
2. Fill every `{{...}}` in `CLAUDE.md` and `context/search-brief.md`.
3. Paste your real experience into `context/profile.md`, your form answers into `context/application-data.md`, and your resume into `context/resume-master.md`. Be specific. The harness can only claim what is written here.
4. Fill `context/claims.md`, especially the **NOT confirmed** list. This is what stops Claude from "matching" you to tools you have never used.
5. Tune the weights in `context/rubric.md` if your priorities differ. Keep the 1/3/5 scale and the quote rule.
6. Connect Gmail in Claude's connectors if you want `/follow-ups` to read replies. Read-only use is all it needs.
7. Run `/find-jobs` once by hand. Read the report. Disagree with something? Run `/calibrate`.
8. Put it on a schedule with `SCHEDULE.md`.

See `examples/example-daily-report.md` for what a run looks like.

## Rules I would not remove

- **No quote, no score.** Without it, the same posting gets a different verdict on different days.
- **Score from the employer's own board.** Aggregators mislabel level, location and pay.
- **The verifier sees files, not reasoning.** A checker that reads the scorer's reasoning will mostly agree with it.
- **Calibration needs your approval.** An AI that rewrites its own rules unwatched drifts.
- **You decide.** The harness never applies, sends or messages on its own.

## License
MIT. Fork it, change it, share what you learn.
