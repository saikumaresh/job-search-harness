---
name: calibrate
description: the user's correction loop. Turns their disagreement with a verdict into one general rule, shows which past verdicts it would change, and saves it only after they approve. Run only when the user types /calibrate.
disable-model-invocation: true
---

# /calibrate {date} {feedback}

Only the user runs this. Never run it from a scheduled task, and never on your own initiative.

## 1. Find the case
Open the scorecard(s) from that date that the feedback is about, and their JD snapshots.

## 2. Diagnose
Say plainly why the verdict came out the way it did: which dimension, which quote, which rubric line or calibration rule drove it, and where the user's view differs.

## 3. Propose one general rule
Write one rule that would have produced the user's verdict, phrased so it applies to future postings and not just this company:
`When {condition in the JD}, score {dimension} as {1, 3 or 5} (or gate as {verdict}) because {the user's reason}.`
A fix for one company is not a rule. If the feedback is really about a missing fact in PROFILE.md, say that instead and suggest the profile update.

## 4. Show the blast radius
Re-score every posting in `memory/jd-cache/` from the last 30 days under the proposed rule, without saving anything. List every verdict that would change, including ones the user may not expect.

## 5. Stop and ask
Show the diagnosis, the rule and the changes. Ask the user to approve, edit or reject. Wait.

## 6. Only after approval
- Append the rule to `memory/calibration.md` with an id (C-{n}), date, the rule, the user's reason, and the example that triggered it.
- Re-score the changed postings for real, update their scorecards, and move them in `memory/tracker.md` where the verdict changed.
- Append one line to `memory/runs.md`.
- Never edit `context/rubric.md` from this skill. The rubric changes only when the user asks for it directly.
