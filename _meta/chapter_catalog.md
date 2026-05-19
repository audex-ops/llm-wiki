<!-- disclosure: public -->

# Chapter Catalog

The complete list of chapter files that may exist inside a `projects/NN-slug/` folder. To introduce a new chapter, edit this file first.

## Folder naming

`projects/NN-slug/` where `NN` is the project number (01–99) and `slug` is lowercase-hyphenated English. Slug is stable; do not rename mid-stream.

## Required chapters (every project has these)

| File | Domain |
|------|--------|
| `00_context.md` | Project identity, period, nature, team composition, author's role label, completion status, chapter map |
| `01_problem_and_goal.md` | The problem being solved, the goals at kickoff, who defined them, any goal changes |
| `02_constraints_and_environment.md` | Stack, infrastructure, data, policy, time, headcount constraints; pre-existing assets |
| `03_what_i_did.md` | Author's actual work, factually. Separated from teammates' work |
| `04_decisions.md` | Decision records: options, choice, reasoning at the time, decision authority |
| `05_outcomes.md` | What was produced. Quantitative results with method/date/scope. Qualitative observations. External evaluations |
| `06_retrospective.md` | Post-hoc judgments. Mistakes identified later. What would be done differently. Unresolved questions |
| `07_evidence.md` | Pointers to raw sources: repos, commits, documents, presentations. Access constraints noted |

## Optional chapters (create only when relevant)

| File | Domain | Use when |
|------|--------|----------|
| `10_team_and_role.md` | Team structure, the author's coordination work, conflicts, communication tools | Team-lead role or heavy collaboration |
| `11_stakeholders.md` | External interfaces: customers, instructors, end users, ops targets | Enterprise-linked, operational, or user-facing project |
| `12_operations.md` | Deployment, monitoring, incidents, handover | The project reached an operational phase |
| `13_data_and_metrics.md` | Data scale, metric definitions, measurement tools, baselines | Data-heavy or performance-measured project |
| `14_artifacts_index.md` | Inventory of diagrams, schemas, API specs and where they live | Many artifacts; `07` becomes too crowded |

## Forbidden chapter types

The following exist nowhere in this library, by design:

- `summary.md`, `highlights.md`, `pitch.md` — summarization is an output-time operation, not a storage form.
- `skills.md`, `competencies.md`, `strengths.md` — evaluative; belongs in output layer, not library.
- `STAR.md` and other interview-format files — output formatting belongs in output layer.

## Empty file policy

Optional chapters: do not create the file unless it has content. A missing file means "not applicable to this project". Required chapters always exist; their interior follows the empty-section policy in `library_conventions.md` §8.
