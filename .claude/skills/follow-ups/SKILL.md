---
name: follow-ups
description: Sweeps the application inbox and the tracker, records replies and stage changes, and drafts follow-up notes for roles that are due. Drafts only, never sends. Use when the user says /follow-ups or a scheduled follow-up run starts.
---

# /follow-ups

## 1. Load
Read `CLAUDE.md`, `memory/tracker.md`, `memory/calibration.md`, and the last `reports/*-follow-ups.md` if one exists (so the window starts where the last run ended).

## 2. Read the inbox (read only)
Search your application inbox (set in `CLAUDE.md`) from the last run date to now for mail from, or about, every company in the tracker, plus ATS senders (Greenhouse, Lever, Ashby, Workday, Wellfound, LinkedIn) and words like interview, assessment, next steps, unfortunately, offer. Read the matching threads. Never reply, forward, label, archive or delete.

## 3. Update stages
Classify each reply: interview or call, assessment, rejection, auto-acknowledgement, or other. Update the role's stage and next action in `memory/tracker.md`. Rejections become REJECTED with the date. Anything needing the user within 48 hours goes at the top of the report.

## 4. Find what is due
A follow-up is due when a role is APPLIED for 7 or more days, has no human reply, and has not been followed up yet. Past 21 days with nothing, suggest GHOSTED instead.

## 5. Draft
For each due role, draft a note of 90 words or fewer: plain text, one specific reason this role and the user fit (from the scorecard), one ask. The recipient comes from the tracker, the posting or the inbox. Never guess a name or an email address. If there is no known recipient, say so and suggest where the user could find one.

## 6. Verify
Send the drafts and the tracker path to the **verifier** agent. Fix every ISSUES item.

## 7. Report and stop
Write `reports/{YYYY-MM-DD}-follow-ups.md` using `context/templates/follow-ups-report.md`. Append one line to `memory/runs.md`. In chat: replies first, then the drafts. Nothing is sent. If the user says "send {company}" in an interactive session, create a Gmail draft the user can send, or send it only if they explicitly ask.
