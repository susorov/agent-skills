# Security workflow

For backend/API work, load the skill for each applicable stage:

- Before changes to service topology, trust boundaries, infrastructure
  exposure, or sensitive data flows: read
  `skills/security/security-architecture/SKILL.md`.
- Before adding endpoints or changing contracts or access policies: read
  `skills/security/secure-api-design/SKILL.md`.
- When changing backend code, security configuration, or runtime dependencies:
  read `skills/security/secure-implementation/SKILL.md`.
- Before completing security-sensitive changes or when reviewing security:
  read `skills/security/security-review/SKILL.md`.

Reuse applicable design decisions across stages. Scope checks to the changed
behavior and affected trust boundaries; documentation-only or formatting-only
edits do not require the backend workflow. Missing evidence remains unverified.
