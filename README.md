# Security agent skills

Seven skills cover backend architecture, API contracts, implementation, and review,
plus frontend, mobile, and Docker/Docker Compose security across the development workflow.
Edit their sources under `skills/security/`. Ignored local links under
`.agents/skills/` provide discovery when installed; `AGENTS.md` selects applicable stages.

## Use in another repository

Copy the seven skill directories together into that repository's `.agents/skills/`
directory, including their references and preserving their names and layout.
Backend review reads the Spring checklist from its sibling `secure-implementation`
directory, so keep both installed.
Merge the stage-selection rules from this repository's `AGENTS.md` into the
target repository's instructions, adjusting paths to `.agents/skills/`.

In a fresh Codex session, check that all seven skills appear in the skill selector
and explicitly invoke one before relying on implicit selection. Installation
does not prove correct routing or security effectiveness.

Baselines are OWASP API Security Top 10:2023, OWASP Top 10:2025, selected
ASVS 5.0.0 requirements, and selected MASVS 2.1.0 controls. Apply checks only
to relevant features and detected stack versions; record exclusions and evidence.
Container checks use Docker and OWASP guidance, with relevant NIST SP 800-190
recommendations; select CIS Docker Benchmark requirements for the detected runtime
when applicable without implying a complete compliance assessment.

References: [Codex skills](https://learn.chatgpt.com/docs/build-skills),
[OWASP API Security](https://api-security.owasp.org/editions/2023/en/0x11-t10/),
[ASVS 5.0.0](https://github.com/OWASP/ASVS/tree/v5.0.0),
[OWASP Top 10:2025](https://top10.owasp.org/2025/),
[MASVS 2.1.0](https://github.com/OWASP/masvs/releases/tag/v2.1.0).
