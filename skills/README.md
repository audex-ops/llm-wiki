<!-- disclosure: public -->

# Skills

Four skills. Run them in this order for a new project:

1. `extract-from-repo` — pre-fill chapters that code and git can answer.
2. `interview-user` — fill the rest by asking the user.
3. `validate-library` — check for contradictions, missing measurements, evaluative language, sensitivity violations.

For producing application materials from the library:

4. `compose-output` — read-only over the library; produces drafts elsewhere.

## How to invoke

Open Claude Code with this library as the working directory. Then in chat:

- "Run extract-from-repo on project 01 with repo at /Users/me/repos/foo." → Claude reads `skills/extract-from-repo/SKILL.md` and follows it.
- "Interview me about project 01." → Claude reads `skills/interview-user/SKILL.md`.
- "Validate the whole library." → Claude reads `skills/validate-library/SKILL.md` with scope=all, mode=report.
- "Draft a 1000-character Korean essay answer for [question text], based on project 01 and 03." → Claude reads `skills/compose-output/SKILL.md`.

If Claude Code is configured with a `CLAUDE.md` at this library root, add a line pointing to `skills/` so each session discovers the skills automatically.

## Tooling assumed

- `bash` (for `git log`, file listing)
- `read`/`write` access to library files
- Optional: GitHub MCP for remote repo metadata. Not required.

## What these skills are NOT

- They are not autonomous agents. Each skill stops at well-defined gates (after extract → user reviews; after interview batch → user answers).
- They do not produce evaluative content under any prompt. Forbidden words are forbidden regardless of how the user asks.
- They do not modify `_meta/profile.md` or `_meta/sensitivity_rules.md`. Those are user-edited only.
