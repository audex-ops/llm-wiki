# Skill: extract-from-repo

## When to use

When the user wants to populate a project chapter set from a code repository (git repo, local or remote). Run this **before** `interview-user` so that the interview targets only what code cannot answer.

Do NOT use this skill to write evaluative content. It extracts facts only.

## Inputs required from the user

1. Path to the local clone of the repository (or URL to clone first).
2. Project slug, e.g. `01-samsung-rf-pipeline`.
3. The author's git identity in this repo: name and/or email used in commits. Multiple identities allowed.
4. Approximate project period (start ~ end), to scope `git log`.
5. NDA / sensitivity status: is the repo content allowed to be sent to Anthropic API? If no, the skill operates in `outline-only` mode (described below).

If any input is missing, ask once with all missing items in a single ask_user_input call.

## Source priority

When facts conflict between sources, resolve in this order (highest to lowest):

1. **Verifiable code / git artifacts** — source code, schema files, migration files, build configs, commit history, `git log`, manifest files. Authoritative — these either are or are not in the repo, and the skill can re-check them at any time.
2. **User chat in the same conversation** — interview-grade. When the user provides context (project background, lifecycle, decisions, retrospectives, named SHAs, outcomes), treat it as a primary fact source and write it into chapters tagged `[fact] [user-supplied]`. Do not defer this content to a separate `interview-user` pass when the chat has already supplied it.
3. **Repo `README`, `docs/`, in-repo wiki, doc comments** — secondary. These may be post-hoc, aspirational, drifted from current code, or describe abandoned plans. The README is read for orientation only; it does not get to dictate facts.

When repo docs conflict with user chat, the user wins. Record the discrepancy as "the README claims X but the user states Y; user-supplied is treated as authoritative", not as "open question to resolve later".

When repo docs conflict with code / git, code wins. If `README.md` claims a `KafkaConsumer` but the repo has no Kafka dependency, the README is wrong.

Tag annotations reflect the source: `[auto-extracted: <path or command>]` for sources of type 1; `[user-supplied]` for type 2; `[auto-extracted: README]` is permitted only when the README claim is also corroborated by type 1 or type 2.

## Operating modes

### Mode A: full (default)

The skill reads source files directly. Used when the repo is non-sensitive.

### Mode B: outline-only

The skill only reads file paths, directory structure, package manifests, CI config, README, and `git log --stat`. It does NOT read source file contents. Used for NDA-protected repos. Outputs will have lower density and more `[estimate]` tags.

User declares the mode in the input. If unspecified, default to `full` but ask for confirmation when an enterprise project name appears.

## Procedure

For each step, the skill writes results into the target project folder `projects/<slug>/`. Every claim is tagged `[fact]` or `[estimate]`. Every claim from this skill also receives an `[auto-extracted: <source>]` annotation citing the file/command it came from.

### Step 1. Repo shape → `00_context.md` (partial) and `02_constraints_and_environment.md` (technical environment)

Read:
- `README.md`, `README.*`
- Root directory listing (depth 2)
- Package manifests: `package.json`, `pom.xml`, `build.gradle`, `requirements.txt`, `pyproject.toml`, `Cargo.toml`, `go.mod`
- Dockerfile, `docker-compose.yml`, `.dockerignore`
- CI: `.github/workflows/*`, `.gitlab-ci.yml`, `Jenkinsfile`
- Infrastructure: `terraform/`, `k8s/`, `helm/`, `deploy/`, `*.tf`, `*.yaml` under infra paths

Write to `02_constraints_and_environment.md`:
- "Technical environment" subsection — languages, frameworks, infrastructure components with versions from manifests. Tag each line `[fact] [auto-extracted: <manifest path>]`.

Write to `00_context.md`:
- "Team composition" — leave for interview, do not guess.
- "Period" — fill from `git log --reverse --format=%ai | head -1` and `git log -1 --format=%ai`. Annotate as `[fact] [auto-extracted: git log]`.

### Step 2. Authorship → `03_what_i_did.md` and `00_context.md` team field cross-check

Run:
- `git log --author="<author>" --no-merges --stat` (scoped to the period).
- `git log --no-merges --pretty=format:"%an"` | sort | uniq -c → contributor list.
- `git log --author="<author>" --no-merges --pretty=format:"%H %s"` → commits with subjects.

Write to `03_what_i_did.md`:
- "Work performed directly by the author" subsection. Group commits by directory area. For each area: directory path, commit count, line-change totals, and a one-line description derived from frequent words in commit subjects.
- Each entry tagged `[fact] [auto-extracted: git log filtered by author]`.

Write to `00_context.md`:
- Update "Team composition / headcount" from the unique-contributors count (mark `[estimate]` since git identities may not match real headcount).

### Step 3. Architecture → `13_data_and_metrics.md` and `14_artifacts_index.md`

Mode A only (skip in outline-only mode):
- Identify entry points (main classes, `app.py`, `server.ts`, etc.).
- Build a high-level import graph at module level (not function level). Output as a `mermaid` diagram in `14_artifacts_index.md` under a "Generated diagrams" subsection.
- Detect database schemas: migration files, `schema.sql`, ORM model files. Summarize entity names and key relations into `13_data_and_metrics.md` under "Data scale and shape". Tag `[fact] [auto-extracted: <migration file path>]`.
- Detect API surface: routers, controllers, OpenAPI files. List endpoint paths and methods in `14_artifacts_index.md` under "API surface".

Mode B: skip this step. Note in `14_artifacts_index.md`: `pending — repo content not analyzed under NDA mode`.

### Step 4. Operations signals → `12_operations.md` if relevant

If Dockerfile, k8s manifests, terraform, or CI deploy stages exist:
- Create `12_operations.md` (if not already present).
- "Deployment environment and method" subsection — derived from these files. Tag `[fact] [auto-extracted: <path>]`.
- Leave "Monitoring", "Incidents", "Handover" empty (mark `pending`) — interview-user will fill.

If no such signals exist, do NOT create `12_operations.md`.

### Step 5. Evidence pointers → `07_evidence.md`

- Repository URL or local path.
- Top 10 PRs/commits by line change in the author's commits, with subject and SHA.
- README sections that document the project.
- Any `docs/` directory contents listed.

### Step 6. Cross-project index update

Update `cross_project_index.md` at library root:
- Technology Matrix: mark the technologies detected.
- Project Roster: status, nature.
Do not overwrite manually-set values; only fill blanks.

### Step 7. Log entry

Append to `_log.md`:
```
## [<today>] extract | <slug> | mode=<A|B>, files_read=<N>, commits_scanned=<N>, chapters_touched=<list>
```

## Output to the user (in chat)

After Step 7, summarize to the user:

1. Mode used.
2. Chapters touched and a one-line summary per chapter.
3. Specific items the skill chose NOT to fill, and why (those will be the input to interview-user).
4. Items the skill marked `[estimate]` — list them for user verification.

End with: "Ready for `interview-user`. Continue?"

## Hard rules

- Never write `[fact]` for anything not directly sourceable from a file or command output cited in the same line.
- Never write evaluative language. If commit message says "fix critical bug", the skill writes "fixed bug labeled critical by author at commit time" not "resolved critical issue".
- Never invent team member names. Use roles only.
- If the user's identity (git author) matches < 5% of commits in the scoped period, stop and confirm with the user before proceeding — likely wrong identity provided.
- Respect `_meta/sensitivity_rules.md` if present. Apply project-specific overrides before writing.
- Disclosure level for files created/modified by this skill: `interview-only` (default).
- **Never treat a repo `README` / `docs/` claim as `[fact]` standalone.** Corroborate via code, git, or user chat. See "Source priority" above.
