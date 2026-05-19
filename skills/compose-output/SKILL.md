# Skill: compose-output

## When to use

To produce an output artifact (resume bullet set, cover letter / Korean 자기소개서 answer, portfolio website section, slide content, interview-prep answer outline) by drawing from the library.

This skill READS the library. It does NOT write into chapter files. New facts surfaced during composition are routed through `interview-user`, not through direct edits here.

## Inputs

1. **Target type**: `resume-bullets`, `kr-essay-answer` (Korean 자기소개서 문항), `portfolio-section`, `slide-content`, `interview-prep`, or `custom`.
2. **Target spec**:
   - For `kr-essay-answer`: the exact question text, character limit, company name and human profile (인재상) if available.
   - For `resume-bullets`: target role, JD text or summary.
   - For `portfolio-section`: section name, audience (recruiter / engineer / generalist).
   - Others: free text.
3. **Project scope**: which project(s) to draw from (slugs), or `auto` to let the skill choose.
4. **Disclosure ceiling**: highest level allowed in the output. Default: `interview-only` for interview prep, `public` for written deliverables. Skill refuses to include `internal-only` content.
5. **Language**: `ko` or `en`. Library content may be mixed; skill translates as needed.

## Procedure

### Step 1. Select source material

Read `cross_project_index.md` and each candidate project's `00_context.md` (chapter map). Pick the project(s) whose nature, technology, or domain best matches the target. If `auto` and the match is unclear, ask the user with `ask_user_input_v0`.

### Step 2. Pull facts

From the selected project(s), read the specific chapters needed for the target:

| Target | Primary chapters | Secondary |
|--------|------------------|-----------|
| resume-bullets | 03, 05 | 04, 13 |
| kr-essay-answer | depends on question theme — see Step 2b | all |
| portfolio-section | 00, 02, 13, 14, 05 | 03, 04 |
| slide-content | 00, 02, 13, 05 | 03 |
| interview-prep | 04, 06 | 03, 05 |

#### Step 2b. Korean essay question → chapter mapping

Common 인사이트-side question patterns:

- "지원동기 / 입사 후 포부": 00, 02 (technical fit), 01 of relevant projects, plus profile context. Tone: forward-looking. Hardest to do without evaluative language — see Hard rules.
- "성장 과정 / 본인 강점": forbidden as worded. Reframe internally to "concrete situations and actions": draw from 03, 04, 05, 06. If the question literally asks for "strength", select 2-3 observable patterns across projects and present as observed behavior, not as identity claims.
- "협업 / 갈등 경험": 10, 04 (decisions made in disagreement), 06.
- "도전 / 실패 / 어려움 극복": 04 (decision under constraint), 05 (outcome), 06 (retrospective). Honest failures preferred over packaged ones.
- "기술적 문제 해결": 04, 13, 05. The decision and its measured effect.

### Step 3. Collect with citations

For each fact pulled, record: `<project-slug>/<chapter>:<line range>` as an internal citation. The draft output carries these citations as comments at end of each sentence/bullet. The user sees them; they are stripped from the final shareable version on request.

### Step 4. Draft

Compose the output. Constraints:

- **No evaluative language not present in the source.** If the source says "the pipeline processed N records/sec, measured by X on Y, against baseline Z", the output may say "처리량 N records/sec (X 도구 기준, Y일 측정, 기존 Z 대비)". The output may NOT say "고성능", "효율적", "탁월한".
- **No facts not present in the source.** If composing in Korean and a phrasing in Korean implies a fact the library does not contain, rewrite.
- **Character limits** strictly respected for Korean essays.
- **Disclosure ceiling** strictly respected. If a needed fact exceeds the ceiling, leave a gap and tell the user.

### Step 5. Surface gaps and feedback

After producing the draft, output a "gap report":
- Facts the target would benefit from but the library does not contain.
- Sentences that came close to evaluative language and where rewritten cautiously.
- Claims that crossed disclosure ceiling and were dropped.

Ask the user: "Should any of these gap items go to `interview-user` to be added to the library?"

If yes, signal that the next session should run `interview-user` on the relevant project.

### Step 6. Log

Append to `_log.md`:
```
## [<today>] compose-feedback | <target type> | drew from <slugs>, <N> gap items surfaced
```

This entry exists so feedback can be tracked back. The composed output itself is NOT stored in the library.

## Hard rules

- The skill outputs to chat or to a file outside the library (e.g. user's working directory). Never writes inside `asset-library/`.
- Self-referential identity claims that the library does not factually establish are forbidden. The skill is allowed to say "led a 6-person team" only because `00_context.md` records this; not allowed to say "natural leader".
- For 인재상 / company values matching: the skill matches by *evidence in library facts*, not by adding adjectives. If the company values "도전" and a project decision in `04` shows the author chose a riskier option with documented reasoning, that fact is used. If no such fact exists, the skill says so rather than inventing one.
- Disclosure: `compose-output` reads disclosure headers and silently drops chapters/sections above the ceiling. It MUST disclose this in the gap report.
- Length discipline: Korean essay char limits are exact. Skill counts characters (Hangul = 1 char each in standard 자기소개서 counts) before declaring done.
