# Claude Code Configuration for this Library

This is an asset library (see `README.md`). When working inside this directory, follow these rules.

## Discovery

The directory `skills/` contains skill specifications. Before performing any operation on library content, check whether a relevant skill exists in `skills/` and read its `SKILL.md`. The four skills are:

- `extract-from-repo/SKILL.md`
- `interview-user/SKILL.md`
- `validate-library/SKILL.md`
- `compose-output/SKILL.md`

## Conventions

`_meta/library_conventions.md` is the authoritative rule set for content. Read it before writing into any file under `projects/`. Violations are bugs.

`_meta/sensitivity_rules.md` is the authoritative rule set for what content is permitted. Apply project-specific overrides.

`_meta/chapter_catalog.md` defines the only file types permitted under `projects/<slug>/`. Do not create ad-hoc files.

## Forbidden behaviors

- Do not write evaluative or marketing language into library files. The forbidden vocabulary is enumerated in `library_conventions.md` §1.
- Do not paste raw source content (code, document text, recording transcripts) into chapter files. Cite locations in `07_evidence.md`.
- Do not write into the library while `compose-output` is the active skill. Composition is read-only over the library.
- Do not modify `_meta/profile.md` (it is gitignored and user-owned).

## Required behaviors

- Tag every nontrivial claim `[fact]`, `[estimate]`, or `[retrospective]`.
- Add `<!-- disclosure: ... -->` as the first line of any new file.
- Append a line to `_log.md` for every structural change or skill run.

## Language

- Skill files, conventions, file headers: English.
- User-supplied content (chapter body) may be Korean or English. Skills handle either. Korean chapter content stays Korean when consumed by `compose-output`.

## When in doubt

Stop and ask the user. The defaults in this library prefer "less written, more accurate" over "more written".
