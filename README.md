# Backend security skills

Four skills cover architecture, API contracts, implementation, and review.
Edit their sources under `skills/security/`. Repository-local links under
`.agents/skills/` make them discoverable; `AGENTS.md` selects applicable stages.

## Use in another repository

Copy the four skill directories together into that repository's `.agents/skills/`
directory, preserving their names and layout. Review reads the Spring checklist
from its sibling `secure-implementation` directory, so keep both installed.
Merge the stage-selection rules from this repository's `AGENTS.md` into the
target repository's instructions, adjusting paths to `.agents/skills/`.

In a fresh Codex session, check that all four skills appear in the skill selector
and explicitly invoke one before relying on implicit selection. Installation
does not prove correct routing or security effectiveness.

The baseline is OWASP API Security Top 10:2023 and selected ASVS 5.0.0
requirements. Apply checks only to relevant features and detected stack versions.

References: [Codex skills](https://learn.chatgpt.com/docs/build-skills),
[OWASP API Security](https://api-security.owasp.org/editions/2023/en/0x11-t10/),
[ASVS 5.0.0](https://github.com/OWASP/ASVS/tree/v5.0.0).
