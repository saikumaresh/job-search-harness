---
name: prep-application
description: Builds a verified application kit for one role the user picked from the pipeline (tailored one-page resume, cover note, screening answers), then stops for the user's yes. Use when the user says /prep-application with a company and role, or asks to prepare an application.
---

# /prep-application {company} {role}

## 1. Confirm the role
Find it in `memory/tracker.md` and its scorecard in `reports/scorecards/`. It must be GREEN or AMBER. If it is not in the pipeline, run the /find-jobs scoring steps on it first. Re-open the posting: if it is closed, stop and say so. If the JD snapshot is older than 7 days, save a fresh one.

## 2. Load the facts
Read `context/profile.md`, `context/application-data.md`, `context/claims.md`, `memory/calibration.md`, and the tailoring notes in `context/search-brief.md`. These are the only sources of facts about the user.

## 3. Build the resume
- Start from your master resume (`context/resume-master.md`, or your own builder: LaTeX, Docs, anything). Make a copy for this role. Mirror the JD's vocabulary only where it is true. Put the answer to the JD's top requirement in your first bullet.
- Save it as `applications/{Company}_{Role}_resume.pdf` (or .docx).
- Check it is one page. Trim until it is.
- List the JD's keywords, mark each as covered truthfully or missing, and compute the match. Aim high, with true claims only. Mapping lines (for example "EMR Spark, Databricks-equivalent") are fine. Invented hands-on experience is not.

## 4. Write the words
- Cover note: three short paragraphs, under 200 words, no em dashes. Why this company, the one most relevant thing the user built, what they want to do there.
- Screening answers: only from `context/application-data.md`. If a question has no source, leave it blank and list it under "Fields left blank on purpose".

## 5. Verify
Send the resume PDF text, the cover note, the answers and the JD snapshot path to the **verifier** agent. It checks that every claim traces to a source, that the resume is one page, that there are no planned certifications, nothing from the private names list, and no em dashes. Fix every ISSUES item.

## 6. Hand over
Write the kit to `applications/KIT_{Company}_{Role}.md` using `context/templates/application-kit.md`. Set the role to stage READY in `memory/tracker.md`. Show the user the kit summary and the resume.

## 7. Stop
Wait for the user's explicit "yes, apply to {company} {role}". Only then, and only in an interactive session, follow the apply gate in `CLAUDE.md`: open the form, fill it, show them the filled form, and submit only after their second yes. Log APPLIED in the tracker the same day. A scheduled run never gets past step 6.
