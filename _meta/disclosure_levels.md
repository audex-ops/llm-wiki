<!-- disclosure: public -->

# Disclosure Levels

Three levels. Every file declares one.

## Definitions

- **public** — content may be quoted verbatim in materials shown to third parties (resume, portfolio website, public presentation).
- **interview-only** — content may be discussed orally in an interview but not quoted in writing or screen-shared. The default for new content.
- **internal-only** — for the author's reference only. Never used in any external material.

## Declaration syntax

First line of every markdown file:

```
<!-- disclosure: public -->
```

The skill `validate-library` will flag any file missing this header.

## Per-section override

If a single file mixes levels, add a header within the file:

```markdown
## Section title
<!-- disclosure: internal-only -->
...
```

The narrower scope wins. Prefer splitting the section into its own file if it gets long.

## Assignment

The author assigns the level when creating or last editing a file. Promotion to a more open level requires a deliberate review pass.

## Skill behavior

`compose-output` reads disclosure levels and refuses to include `internal-only` content in any output, and includes `interview-only` content only when the user explicitly confirms the target is an interview context.
