<!-- disclosure: public -->

# Sensitivity Rules

Defaults for what content is permitted in library chapters and what requires transformation before inclusion. Project-specific overrides go in the project's `00_context.md` under a "Sensitivity overrides" subsection.

These rules are enforced (with a flag-and-suggest model, not auto-delete) by `validate-library`'s F-check.

## 1. Names of individuals

- **Author's own name and handles**: allowed at any disclosure level. Author chooses what to expose.
- **Teammate names and handles**: by default, redact to role labels in chapters that may flow to public outputs (`00`, `03`, `07`, `10`). Examples: "AI-server primary teammate", "backend co-lead", "QA contributor". Keep the redacted form consistent across chapters of the same project.
- **Customer or end-user names**: do not include. Reference by role or by anonymized identifier.
- **Reviewers / interviewers / instructors mentioned by name**: redact to role unless they themselves have publicly attached their name to the project (e.g., a course evaluator who signed an award certificate that is itself public).

## 2. Internal codenames

- **Public product names**: allowed.
- **Internal-only codenames** (project names not used externally): transform to a stable pseudonym per project, declared in `00_context.md`. Once declared, use the pseudonym consistently across all chapters.
- **Component / service names that imply internal architecture**: case-by-case. If the name itself reveals confidential design or business logic, transform.

## 3. Specific numbers from enterprise / NDA projects

- **Numbers with a public source** (press release, public dashboard, published case study, public GA property): allowed; cite the public source in `07_evidence.md`.
- **Numbers without a public source**: do not include exact values. Substitute order-of-magnitude or relative descriptors ("six-figure", "~30% reduction") only if those are themselves defensible from a non-confidential source.
- **Numbers from a personal / open-source / publicly operated project**: allowed with method, date, and scope per `library_conventions.md` §3.

## 4. Disclosure mismatch

- A `public` file must not contain content that exists only in an `internal-only` source elsewhere.
- A `public` file's claims must be defensible without reference to private sources.
- When in doubt, raise the file's disclosure level (e.g., `public` → `interview-only`) rather than risk exposure.

## 5. Project-specific overrides

Each project may declare additions or relaxations to these defaults in its `00_context.md` under a "Sensitivity overrides" subsection. Project-level rules take precedence over these defaults *for that project's chapters only*.

If a project's overrides are stricter than the defaults, the stricter rule wins. If looser (e.g., teammates have given explicit written consent to be named on a public portfolio), the project must record the rationale and the source of the permission in `00_context.md`.

## 6. Out-of-scope content (never in this library, regardless of disclosure)

- Production credentials, API keys, OAuth tokens, signing keys.
- PII of end users (real names, emails, IPs, geolocation, payment data) — synthetic, aggregated, or anonymized only.
- Confidential business numbers without a public-source justification (per §3).
- Communications that were marked confidential by their sender (Slack, email, internal docs): paraphrase the substance without attribution; do not include verbatim text or sender identity.

If unsure: leave it out, write `pending — sensitivity review required`, and surface the decision to the user explicitly.
