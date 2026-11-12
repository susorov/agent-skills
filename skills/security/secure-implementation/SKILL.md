---
name: secure-implementation
description: >
  Implement secure backend and API code.
  Use when writing or modifying endpoints, services,
  middleware, database access, authentication,
  file handling, external integrations, security configuration,
  or runtime dependencies.
---

# Secure Implementation

Write secure code by default using language and
framework-specific security best practices.

For Spring Boot, read the applicable checks in
[the shared Spring checklist](references/spring-boot.md).

## Requirements

1. **Input & Injection**
   - Validate untrusted input at trust boundaries.
   - Use parameterized database queries.
   - Allowlist query operators and dynamic identifiers; avoid shell interpolation.
   - Constrain file paths to intended roots. For untrusted data,
     avoid native object deserialization and unrestricted polymorphism;
     disable XML external entities when parsing XML.
   - Map allowlisted writable fields explicitly; filter response fields
     by permission rather than exposing persistence objects directly.
   - Encode output for its destination context.

2. **Authentication & Authorization**
   - Enforce server-side authentication and authorization.
   - Enforce object/field permissions and tenant isolation across
     reads, writes, lists/bulk operations, caches, and jobs as applicable.
   - Deny access by default.
   - Keep roles/ownership server-controlled; validate selected tenants
     against authenticated membership.
   - Validate self-contained token signatures, allowed algorithms,
     trusted issuers/keys, audience/purpose, and time claims before use.
   - Use framework authentication/password hashing and implement
     documented session/recovery controls when applicable.

3. **Sensitive Data & Secrets**
   - Keep secrets out of source, build artifacts, and logs;
     use managed runtime sources and the defined key lifecycle.
   - Use secure cryptographic libraries and primitives.
   - Protect sensitive data in transit and at rest
     according to security requirements.
   - Return safe errors without exposing internals.
   - Record authentication events and denied access without credentials
     or sensitive payloads; test error/log redaction.

4. **Framework & Dependencies**
   - Inspect build files, resolved dependencies, and active profiles
     to identify the stack, versions, and effective settings.
   - Apply relevant framework-specific protections.
   - Avoid unsafe APIs and insecure defaults.
   - Prefer maintained dependencies and secure settings.

5. **Resource & Concurrency Safety**
   - Enforce designed rate limits, request size limits, and timeouts.
   - Bound expensive operations and external calls.
   - Prevent relevant race conditions and replay attacks.
   - Use atomic operations for security-critical updates.

6. **External APIs**
   - Restrict outbound schemes, hosts, ports, and resolved addresses
     to intended destinations at connection time; disable redirects or
     revalidate each hop.
     Use egress restrictions where applicable; never forward credentials
     to an unapproved destination.
   - Verify TLS certificates/hostnames, validate upstream schemas,
     and bound response sizes, timeouts, retries, and concurrency.

## Workflow

1. Read existing architecture/API security decisions; identify missing
   requirements and record necessary deviations.
2. Identify untrusted inputs and sensitive operations.
3. Implement with secure defaults and least privilege.
4. Check applicable framework-specific vulnerabilities.
5. For changed security behavior, add and run relevant regression tests:
   allowed access; denied unauthenticated/wrong-role/object/tenant access;
   malformed or privileged fields; and replay/concurrency where applicable.
   Assert denied operations expose no protected data or unauthorized effects.
6. Report enforcing code/configuration, test commands/results, design
   deviations, and unresolved risks. Unrun or blocked checks stay unverified.

## Rules

- Never rely on client-side security controls.
- Do not disable security protections for convenience.
- Preserve existing behavior unless insecure.
- Do not claim security checks passed unless verified.
