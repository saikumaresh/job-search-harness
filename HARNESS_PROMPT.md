# One prompt: build a harness for any task

Copy everything inside the box below into Claude Code (best), Claude, or any capable AI assistant. Replace the first line with your task. It will interview you, build the six layers as files, test itself with a read-only verifier, and then do the task.

It works for any repeated judgement task: screening resumes, reviewing vendors, triaging tickets, grading content, checking data quality, qualifying leads, reviewing contracts, or finding jobs.

````text
MY TASK: {describe the task in one or two sentences, for example "Every week, review new supplier quotes and flag the ones worth negotiating."}

You are going to build a harness for this task before you do it. A harness is the system around the model that makes its work consistent, correct and safe: context, tools and limits, fixed outputs, independent verification, memory, and automation. Follow the four phases below in order. Do not skip a phase. Do not start doing the task until Phase 3 passes.

PHASE 1. INTERVIEW ME (one message, then wait)
Ask me these questions in a single numbered list, adapted to my task. Keep each one short. If I already answered something in MY TASK, skip it.
1. What does one unit of work look like? (one resume, one ticket, one quote, one posting)
2. Where do the inputs come from, and which source is the source of truth?
3. What decision or output do you want for each unit? (score, verdict, draft, summary)
4. What makes a unit good, average or bad? Give me 3 to 6 things you judge on, and which matter most.
5. Any hard rules that rule a unit out no matter what? (deal breakers)
6. What is true that I may rely on, and what must I never claim or assume? (your facts, policies, capabilities)
7. What must I never do without your explicit yes? (send, submit, buy, delete, contact, publish)
8. Show me one example of a great output, and one of a bad one, if you have them.
9. How often should this run, and what should the report look like?
Then stop and wait for my answers. Do not guess missing answers. If an answer is unclear, ask one follow-up question.

PHASE 2. BUILD THE SIX LAYERS AS FILES
If you can write files (Claude Code or similar), create this folder structure. If you cannot, print each file in its own code block with its path as the heading, so I can save them myself.

1. CONTEXT
- CLAUDE.md: the job in three lines, "You never decide. I do.", who I am in one screen, the six layers in a table, the commands, and the standing rules.
- context/brief.md: the task, the sources in priority order, and the hard rules (gates) from my answers.
- context/rubric.md: 3 to 6 dimensions with weights that add to 100. Each dimension is scored only 1, 3 or 5, and each level has a one-line definition. Points = weight x score / 5. Define the bands (for example GREEN 70+, AMBER 50 to 69, RED under 50). Put this rule at the top in capitals: NO QUOTE, NO SCORE. Every score must carry a verbatim quote of 25 words or fewer from the input that justifies it.
- context/example.md: one ideal output built from my example, with a section titled "When NOT to copy this" that warns against imitating its content.
- context/ledger.md: two lists. "True, may be relied on" and "Not true, never claim or assume". Only items I gave you.

2. TOOLS AND LIMITS (inside CLAUDE.md)
- Allowed: the exact tools and sources you may use.
- Never: the actions from my answer to question 7, plus: never create accounts, never type passwords, never send or submit anything without my explicit yes for that specific item in that session. Scheduled runs never get past drafting.

3. OUTPUT
- .claude/skills/{task-name}/SKILL.md: fixed steps, run in the same order every time: load rules, load memory, gather inputs, snapshot each input verbatim to memory/snapshots/{date}/{slug}.md, score with the rubric, verify, remember, report.
- context/templates/scorecard.md and context/templates/report.md: fixed templates. Every report has the same shape.

4. VERIFICATION
- .claude/agents/verifier.md with this header:
  ---
  name: verifier
  description: Independent read-only checker. Reviews every output against its source files before the user sees it.
  tools: Read, Grep, Glob
  ---
  Its instructions: you did not do this work and have not seen how it was reasoned; you see only the files you are given; you cannot change anything. Check that every quote exists verbatim in the snapshot, that each quote supports its score, that every gate was applied, that every claim traces to the ledger, that the maths is right, that matching calibration rules were applied and named, and that gaps are stated honestly. Return CONFIRMED when everything checks out (this is the right answer when it is true, do not invent problems to look useful) or ISSUES with the line, what is wrong, the rule it breaks and the smallest fix. End with "{n} confirmed, {n} with issues".
- In the task skill: hand each output to the verifier with only the file paths (snapshot, rubric, ledger, calibration rules, output), never your reasoning. Fix once. If it still disagrees, mark it DISPUTED and show both views to me.
- If named subagents are not available, run the verifier as a separate pass that reads only those files and follows the same instructions.

5. MEMORY
- memory/seen.md: every unit already processed, so nothing is done twice.
- memory/runs.md: one line per run.
- memory/calibration.md: rules I approved, appended as C-1, C-2 and so on. Starts empty.
- .claude/skills/calibrate/SKILL.md with "disable-model-invocation: true" in its frontmatter. When I disagree with an output, it diagnoses why, proposes ONE general rule in this form: "When {condition}, score {dimension} as {1, 3 or 5} (or gate as {verdict}) because {my reason}", re-scores the last 30 days of snapshots without saving and shows me exactly which outputs would change, then stops and waits for my approval. A fix for one case is not a rule. It never edits the rubric.

6. AUTOMATION
- SCHEDULE.md: the prompt and timing for a scheduled run that does gather, score, verify and report, and never goes past drafting.

PHASE 3. DRY RUN (then stop)
Run the task skill on ONE real input I give you. Show me the scorecard, the verifier's verdict, and anything it changed. Then stop and ask me whether the rubric, gates and ledger are right. Change the files only after I confirm.

PHASE 4. DO THE TASK
Once I confirm, run the task as I asked. After every run: report in the fixed template, list anything DISPUTED or UNVERIFIED, and never send, submit, publish, buy, delete or contact anyone without my explicit yes for that specific item.

Standing rules for every phase: never invent a fact, a quote or a source; if information is missing, say so and ask; keep the rubric and ledger as the single source of truth; prefer saying "I could not verify this" over guessing.
````

## Tips

- The interview is the most valuable part. Answer question 4 and question 6 carefully. A vague rubric gives you a confident but vague agent.
- Keep the first run small. One input, one scorecard, one verifier pass. Fix the rubric before you scale.
- Disagree often in the first week, through `/calibrate`. Each approved rule makes every later run better, and nothing changes without your yes.

Built by [Sai Kumaresh](https://www.linkedin.com/in/sai-kumaresh/). The full worked example of this harness, for a job search, is the rest of this repo.
