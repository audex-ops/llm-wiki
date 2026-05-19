# Skill: interview-user

## When to use

To fill the chapters that code cannot answer: `01_problem_and_goal`, `04_decisions`, `05_outcomes` (qualitative + measured values), `06_retrospective`, and the human side of `10`, `11`, `12`.

Run AFTER `extract-from-repo` for repo-backed projects, so the questions target gaps rather than re-collecting what code already revealed. For non-repo projects (e.g. coursework with no code), run this directly.

## Inputs required from the user

1. Project slug.
2. Whether `extract-from-repo` has been run for this project. If yes, the skill reads the existing chapters before asking.
3. Time budget for this session (e.g. "30 minutes", "as long as needed"). The skill paces its question count accordingly.

## Procedure

### Step 1. Read what already exists

Read every existing file under `projects/<slug>/`. Identify:
- Empty required sections.
- Sections marked `pending`.
- Claims tagged `[estimate]` that need user confirmation.
- Numbers in `05_outcomes.md` missing one of: method, date, scope.
- Contradictions or open ends across chapters (e.g. a decision in `04` referencing an outcome not in `05`).

### Step 2. Build the question list

Categorize questions into four buckets in this priority order:

1. **Verify estimates and auto-extracted items** — the user confirms or corrects facts the previous skill inferred.
2. **Fill required empty sections** — 01, 04, 05, 06 are typically gap-heavy after extract.
3. **Fill applicable optional chapters** — 10, 11, 12 depending on project nature.
4. **Surface retrospective** — only after everything else is in.

Within each bucket, order questions so that an answer can be reused as context for the next question (e.g. ask about decisions before retrospective on those decisions).

### Step 3. Ask in small batches

- Maximum 3 questions per turn.
- Use the `ask_user_input_v0` tool for multiple-choice when the answer space is small and bounded.
- Use free-text questions when nuance matters — write them in chat as numbered questions.
- After receiving answers, write them into the appropriate chapter file IMMEDIATELY, before asking the next batch. This makes long sessions resumable.

### Step 4. Per-answer writing rules

For every answer:
- Strip filler words from the user's reply before writing. Keep the substance.
- Tag the resulting claim `[fact]` (user-stated history), `[estimate]` (user-stated approximation, e.g. "around 2000 RPS"), or `[retrospective]` (user is reflecting from current vantage — goes in 06 only).
- If the user mentions a number, prompt for method/date/scope in the next batch if not provided.
- If the user uses evaluative language ("it was a great experience", "I really grew"), do not write it. Ask: "What concrete observation supports that? I'll write the observation, not the judgment." Then write only the observation.
- If the user mentions a sensitive item flagged by `sensitivity_rules.md`, redirect: "This may fall under sensitivity rules — record it in 06's 'Non-recordable items' section instead?"

### Step 5. Cross-write

When an answer affects more than one chapter, write to all relevant chapters. Example: user says "I picked Kafka over RabbitMQ for partition ordering, but later it caused operational pain we couldn't handle in 3 weeks." → 04 gets the decision; 06 gets the retrospective; 12 may get a note on operational pain (if 12 exists).

### Step 6. Close-out

When the question list is exhausted or the time budget is reached:
- Tell the user what's still pending and at what priority.
- Append to `_log.md`:
  ```
  ## [<today>] interview | <slug> | <N> questions answered, chapters_touched=<list>, pending=<list>
  ```
- Suggest running `validate-library` for this project.

## Question style rules

- One question = one fact. Compound questions get split.
- Open-ended for decisions and retrospectives ("Walk me through the choice between X and Y at <date>").
- Closed-ended for facts the user just needs to confirm or supply ("The repo shows the project ran from Feb 23 to May 20. Confirm?").
- Never lead the user toward a more impressive-sounding answer. If their answer is unimpressive, write it as-is.
- If the user pushes back on a question ("does that matter?"), state which chapter it fills and what output it enables, then let the user skip if they still want to.

## Hard rules

- Never invent details to "fill" a section. Empty stays empty (with `none recorded` or `pending`).
- Never write `[fact]` for hearsay from the user about other team members' work unless the user explicitly confirms direct knowledge.
- Never write retrospective claims into 00–05. Those chapters are the as-of-the-time record.
- If the user contradicts a `[fact]` from `extract-from-repo`, the user wins, but log the contradiction in `_log.md` with `manual-edit` op.
- Disclosure level inheritance: a chapter's existing disclosure level is not changed by this skill unless the user explicitly asks.
