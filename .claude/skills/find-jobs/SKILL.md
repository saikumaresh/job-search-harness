---
name: find-jobs
description: Daily job discovery for the user. Finds new postings, scores each one against the rubric with quoted evidence, has the independent verifier check every scorecard, logs everything to memory and the pipeline, and writes the daily report. Discovery only. Use when the user says /find-jobs, "run the scan", or a scheduled run starts.
---

# /find-jobs

Run these steps in order, every time. Do not skip the verifier. Do not change the report shape.

## 1. Load the rules
Read: `CLAUDE.md`, `context/search-brief.md`, `context/rubric.md`, `context/ideal-role.md` (for format only, read its "When NOT to copy" section), `context/claims.md`, `memory/calibration.md` and `context/profile.md`.

## 2. Load memory
Read `memory/seen-postings.md` and build the set of seen keys. A key is `company | role | req id or URL path`, lower-cased. Read the last five lines of `memory/runs.md` to see which rotating sources ran recently, and pick two or three that did not.

## 3. Search
Find postings from the last 72 hours across the sources in `context/search-brief.md`, using its titles. Cap the run at 40 candidate postings. Skip anything whose key is already seen.

## 4. Snapshot
For each new posting, open it **on the employer's own board** and save the full text to `memory/jd-cache/{YYYY-MM-DD}/{company-role-slug}.md`, with the URL and fetch time at the top. If the employer's posting cannot be read, the verdict is UNVERIFIED and you do not score it.

## 5. Score
Apply the gates, then the five dimensions, exactly as `context/rubric.md` says, with a verbatim quote per dimension. Apply any matching rule in `memory/calibration.md` and name it. Write each scorecard with `context/templates/scorecard.md` to `reports/scorecards/{YYYY-MM-DD}/{slug}.md`.

## 6. Verify
Hand every scorecard to the **verifier** agent (`.claude/agents/verifier.md`), together with the paths to its JD snapshot, `context/rubric.md`, `memory/calibration.md`, `context/profile.md` and `context/claims.md`. Give it nothing else: not your reasoning, not your notes.
- If named agents are not available in this session, start a fresh subagent and give it the full text of `.claude/agents/verifier.md` as its instructions, plus only those paths. Tell it it may only read files.
- For each ISSUES result, fix the scorecard once and send it back. If the verifier still disagrees, mark it DISPUTED and show both views in the report. Never drop a dispute silently.

## 7. Remember
- Append every posting read (all verdicts, including RED and UNVERIFIED) to `memory/seen-postings.md`.
- Add GREEN and AMBER to `memory/tracker.md` at stage FOUND.
- RED roles stay in `memory/seen-postings.md` with the gate or reason.
- Append one line to `memory/runs.md`: date, run type, sources covered, counts, anything blocked.

## 8. Report
Write `reports/{YYYY-MM-DD}-find-jobs.md` using `context/templates/daily-report.md` exactly. In chat, reply with the "At a glance" table, the top three GREEN or AMBER roles in one line each, and the "Needs you" section. Keep the chat reply under 250 words. If nothing worthwhile appeared, say so in one line. Never pad.

## Never, in this skill
Open an application form, click Apply or Easy Apply, save a job, create an alert, message anyone, or invent a posting, salary or requirement.
