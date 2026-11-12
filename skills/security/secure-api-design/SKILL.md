---
name: secure-api-design
description: >
  Design secure REST, GraphQL, and RPC APIs.
  Use when adding endpoints or changing API contracts,
  authentication/authorization rules, or resource access policies.
---

# Secure API Design

Design APIs with security enforced by default.

## Requirements

1. **Authentication**
   - Require authentication unless explicitly public.
   - Use established authentication standards.
   - Define accepted credentials; for self-contained tokens,
     specify trusted issuers/keys, algorithms, audience/purpose,
     and validity checks.
   - When applicable, define password storage, recovery and
     brute-force controls, and session renewal/expiry/revocation.

2. **Authorization**
   - Deny access by default.
   - Enforce endpoint, object, and field-level permissions.
   - Prevent IDOR/BOLA and cross-tenant access.
   - Never trust client-provided roles or ownership; validate
     tenant selection against authenticated membership.

3. **Input & Output**
   - Define strict request and response schemas.
   - Validate types, sizes, formats, and allowed fields.
   - Reject unexpected or unauthorized fields.
   - Never expose secrets or internal error details.

4. **Abuse Prevention**
   - Define rate-limit values, enforcement locations, and applicable
     per-user, per-tenant, and global scopes.
   - Bound timeouts, payloads, and upstream response sizes.
   - Restrict pagination and expensive operations.
   - Protect sensitive workflows against replay,
     enumeration, and race conditions.
   - Define mutation invariants and idempotency scope/lifetime.
   - For GraphQL, bound query cost/depth and batch size.

5. **Transport & Access**
   - Require HTTPS.
   - Restrict CORS to trusted origins when applicable.
   - Protect against CSRF when browsers automatically attach
     credentials, including cookies or browser-managed Basic auth.
   - Never expose internal-only endpoints publicly.

## Workflow

1. Identify actors, resources, and trust boundaries.
2. Define authentication and an actor/action/resource permission
   matrix with allowed and denied cases.
3. Design strict request and response contracts.
4. Identify abuse cases and required protections.
5. Document security assumptions and tradeoffs.
6. Ensure security requirements are testable.

## Rules

- Prefer secure defaults over optional protections.
- Apply relevant OWASP API Security Top 10:2023 risks.
- Never rely on client-side authorization.
- Do not introduce unnecessary attack surfaces.
- Flag missing security decisions before implementation.
- Preserve existing functionality unless changes
  are required to address a security risk.

## Output

For each API design, provide:
- Endpoint and authentication requirements.
- Authorization rules and protected resources.
- Input/output constraints.
- Relevant threats and mitigations.
- Security tests required for implementation.
