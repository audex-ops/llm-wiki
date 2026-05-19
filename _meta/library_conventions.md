<!-- disclosure: public -->

# Library Conventions

These rules apply to every file in this library. Skills enforce them; humans follow them.

## 1. What goes in / what does not

**In:** facts, evidence, structured records of decisions, measured outcomes, post-hoc retrospection clearly labeled as such.

**Out:**
- Evaluative or marketing language: "strength", "differentiator", "growth story", "uniquely positioned", "successfully transitioned", "rare combination", "passion".
- Counterfactual framing: "not just X but Y", "more than a Z".
- Assumptive framing the facts don't support: "as a CS major" if no such fact is established.
- Generic platitudes: "learned the importance of communication".

When in doubt: would a colleague reading this say "that's an observation" or "that's a pitch"? If the latter, rewrite or remove.

## 2. Fact / estimate / retrospective tagging

Every nontrivial claim carries one tag:

- `[fact]` — directly verifiable from code, git, document, recording, measurement, or contemporaneous note.
- `[estimate]` — believed accurate but no primary source available now.
- `[retrospective]` — judgment formed after the fact, based on later information. Only in chapter `06`.

Tagging is at the **claim** level, not the paragraph level. If a sentence mixes a fact and an estimate, split it.

Trivial claims (e.g. "the project lasted three months", already in metadata) need not be tagged.

## 3. Numbers

Every quantitative claim carries:
- the metric definition (what was measured),
- the method (how it was measured — tool, environment),
- the date or window of measurement,
- the scope (which subsystem, which dataset).

If any of the four is missing, write `unmeasured` and leave the number out. Do not write a number you cannot defend.

## 4. Disclosure levels

Every file declares its disclosure level on the first line:
```
<!-- disclosure: public | interview-only | internal-only -->
```

- `public` — can be quoted verbatim in external materials (resume, portfolio).
- `interview-only` — may be discussed orally in interviews; not quoted in writing.
- `internal-only` — for the author's reference; never leaves this library.

Default for new files: `interview-only`. Raise to `public` only after sensitivity review.

## 5. Source-of-truth boundary

Raw sources (code, documents, recordings, evaluation reports) live *outside* this library. `07_evidence.md` in each project points to them by path or link. **Do not paste raw source content into library chapters.** Paraphrase and cite the location.

Exception: short verbatim excerpts (under 30 words) of one's own writing, with location reference, are allowed.

## 6. Feedback loop from output composition

When producing outputs (CVs, essays, etc.) via `compose-output`, new facts often surface ("oh, that decision actually happened in week 3"). Those facts flow back to the library through `interview-user`, never by direct edit during output composition. This keeps the library's audit trail clean.

## 7. In-progress projects

If a project is still ongoing on the day a chapter is being filled:
- `00_context.md` status field: `In progress (as of YYYY-MM-DD)`
- `06_retrospective.md`: rename to `06_retrospective_draft.md`. Keep this rename until the project is genuinely complete. Then rename back, do a full pass.
- `05_outcomes.md`: mark outcomes that are not yet measurable as `pending`.

## 8. Empty sections

Never leave a section empty. If there is nothing to record, write one of:
- `not applicable` — the section's topic does not apply to this project
- `none recorded` — the topic applies but no fact was captured
- `pending` — the fact exists but is not yet documented (must come back to it)

This makes "forgot to fill" distinguishable from "intentionally empty" at audit time.

## 9. Chapter file additions

Project chapters are limited to those defined in `chapter_catalog.md`. To add a new chapter type, edit the catalog first, then add to projects. Do not create ad-hoc files inside `projects/NN-slug/`.

## 10. Self-references

The author refers to themselves consistently — see `profile.md` for the chosen form. Skills follow whatever convention is in profile.
