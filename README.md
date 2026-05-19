# Asset Library

A structured record of project experiences. Not a portfolio, not a resume, not a self-introduction. **A source layer.** Outputs (portfolio website, resume, application essays) are produced elsewhere by reading from this library.

## What this is

- Facts, evidence, and insights about each project — recorded once, structured the same way every time.
- Read by humans and by Claude Code skills that extract relevant fragments at output time.

## What this is NOT

- Not a place for evaluative language ("strengths", "differentiators", "growth", "unique").
- Not a place for polished narrative.
- Not a place where outputs (CVs, essays, slides) are stored or generated.

## How to read this directory

1. Start here (`README.md`).
2. Read `cross_project_index.md` to see which projects cover which technologies/domains.
3. Open the relevant `projects/NN-slug/` folder. Read `00_context.md` first — it has a `Chapter Map` pointing to where the facts live for that project.
4. Read the specific chapters you need.

## How to fill this

Use the skills in `skills/`:

- `extract-from-repo` — read a git repository and pre-fill chapters that code/git can answer (architecture, stack, who-touched-what, timeline).
- `interview-user` — read the pre-filled draft and ask the user the questions that code cannot answer (decisions, outcomes, retrospective).
- `validate-library` — check for contradictions, missing measurements, untagged speculation.

Run them in that order, per project. See each skill's `SKILL.md` for usage.

## How to produce outputs from this

Use the skill `compose-output`. Inputs: a target (job posting, essay prompt, portfolio section) + selection criteria. Output: a draft that cites which library chapters/lines were used. **The skill does not modify library contents.** New facts surfaced during output composition flow back via `interview-user`, not directly.

## Conventions in one paragraph

Every claim in this library is tagged `[fact]`, `[estimate]`, or `[retrospective]`. Numbers always carry a measurement method and date. Sensitive content follows `_meta/sensitivity_rules.md`. Every file declares a disclosure level (`<!-- disclosure: <level> -->`). Full details: `_meta/library_conventions.md`.

## Change log

Library structure changes are recorded in `_log.md`. Project content changes are not — those are tracked by git on the library itself.
