# Security workflow

For security-relevant browser or mobile work, load the applicable client skill:

- Browser authentication, untrusted rendering, API integration, routing, storage,
  security configuration, or third-party code: read
  `skills/security/frontend-security/SKILL.md`.
- Android/iOS authentication, storage, networking, permissions, platform integration,
  SDKs, or release configuration, including Flutter and React Native: read
  `skills/security/mobile-security/SKILL.md`.

These skills cover client design, implementation, testing, and review. For backend
work within the task (including review), load the applicable stages below;
otherwise report required backend controls as integration requirements.

For Dockerfile, Docker Compose, image/build, or associated runtime security work,
read `skills/security/container-security/SKILL.md`. Use the backend stages below
when the task also involves application behavior or architecture; keep cluster
administration and unrelated host changes outside the container workflow.

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
behavior and affected trust boundaries; documentation-only, formatting-only,
or purely cosmetic edits do not require this workflow. Missing evidence remains
unverified; do not claim a control passed because it was merely specified.
