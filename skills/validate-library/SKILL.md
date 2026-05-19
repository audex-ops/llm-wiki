# Skill: validate-library

## When to use

After `extract-from-repo` and `interview-user` have completed for a project. Also run periodically across the whole library when adding new projects, to catch drift.

## Inputs

- Scope: a single project slug OR `all` for the whole library.
- Mode: `report` (print findings, change nothing) or `fix` (propose edits, ask before applying each).

## Checks performed

### A. Structural

1. Every required chapter exists (`00`–`07`) for every non-placeholder project.
2. Every file starts with a `<!-- disclosure: ... -->` line.
3. Optional chapter files exist only if they have content (catch empty optional files).
4. No ad-hoc files inside `projects/<slug>/` outside the chapter catalog.
5. Folder slug is lowercase-hyphenated.

### B. Tagging discipline

1. Every nontrivial claim carries `[fact]`, `[estimate]`, or `[retrospective]`. Heuristic: any sentence stating a non-meta property of the project or its outcomes.
2. `[retrospective]` only appears in chapter `06`.
3. `[auto-extracted: ...]` annotations cite a real path or command (sanity check, not full verification).

### C. Numeric discipline

1. Every number in `05_outcomes.md` and `13_data_and_metrics.md` has metric definition, method, date, and scope.
2. If a number appears in `04_decisions.md` as justification, the same number appears in `05` or `13` with full context.

### D. Cross-chapter consistency

1. Decisions in `04` whose justification rests on a measurement → that measurement appears in `05` or `13`.
2. Outcomes in `05` whose presence implies a decision → that decision appears in `04` (or is explained as not-a-decision).
3. Retrospective items in `06` reference specific decisions/outcomes by chapter and claim. No floating regrets.
4. `03_what_i_did` and `07_evidence` are consistent: claims in `03` about work performed should be traceable to evidence in `07` (commits, docs, etc.).
5. Tech stack in `02` consistent with `cross_project_index.md` technology matrix.

### E. Evaluative language scan

Lint for forbidden words/phrases. Flag, do not auto-delete:
- "strength", "weakness" (as self-attribution), "differentiator", "uniquely", "passion", "passionate"
- "as a CS major" / "as a non-CS major" / similar identity framings if `profile.md` does not establish the fact
- "successfully X", "effectively X", "X significantly improved"
- "rare", "unique combination", "special"
- "grew", "growth" (in self-development sense), "transition" + value-laden modifier

When flagged: show the line, suggest a fact-only rewrite. In `fix` mode, propose and confirm.

### F. Sensitivity audit

1. Names of individuals: flag.
2. Internal codenames: flag if not transformed per `sensitivity_rules.md`.
3. Specific numbers from enterprise projects: flag if no public source.
4. Any disclosure mismatch: a `public` file containing content also present in an `internal-only` source.

### G. Status hygiene

1. In-progress projects: `06_retrospective_draft.md` is used, not `06_retrospective.md`.
2. `00_context.md` status field consistent with file naming (no `Completed` with a `draft` retrospective).
3. `pending` markers older than 30 days: list them as overdue.

## Output

For each finding:
```
[severity] [check id] <file>:<line if applicable> — <description>
  current: <quoted text>
  suggested: <alternative>
```

Severities: `error` (structure or hard rules broken), `warning` (likely an issue), `notice` (style / aging pending).

Group by project, then by chapter, then by check id.

Summary line at the end:
```
<N> errors, <M> warnings, <K> notices across <P> projects.
```

Append to `_log.md`:
```
## [<today>] validate | <scope> | E<N>/W<M>/N<K>
```

## Hard rules

- Never silently rewrite content. Even in `fix` mode, every change is shown and confirmed.
- Never lower a disclosure level on the user's behalf — only raise after explicit user instruction.
- Never delete a file. Suggest deletion; user does it.
