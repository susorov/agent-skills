---
name: security-review
description: >
  Review backend and API code for security vulnerabilities.
  Use during code reviews, before merging or releasing,
  or after changes to authentication, authorization,
  sensitive data handling, or infrastructure.
---

# Security Review

Identify security vulnerabilities using risk-based,
evidence-driven analysis.

For Spring Boot, read the applicable checks in
[the shared Spring checklist](../secure-implementation/references/spring-boot.md).

## Requirements

1. **Authentication & Authorization**
   - Check authentication bypass and privilege escalation.
   - Verify object, field, and tenant-level authorization.
   - Identify IDOR/BOLA and insecure session handling.
   - Verify applicable token signature, algorithm, issuer/key,
     audience/purpose, and validity checks and session lifecycle controls.

2. **Input & Execution**
   - Check injection, SSRF, path traversal,
     unsafe deserialization, and command execution.
   - Identify missing validation and unsafe APIs.
   - Trace outbound destinations through DNS and redirects; verify TLS,
     upstream schema/size limits, bounded retries, and credential isolation.

3. **Data & Secrets**
   - Check sensitive data exposure and insecure storage.
   - Identify hardcoded secrets and unsafe cryptography.
   - Verify safe logging and error handling.

4. **Configuration & Dependencies**
   - Check framework-specific vulnerabilities.
   - Identify insecure defaults and exposed services.
   - Inspect resolved dependencies and advisories; report affected versions
     and applicability evidence. Check runtime/service permissions.
   - Check relevant network and transport controls.
   - Reconcile exposed hosts/routes/versions and production profile settings
     with the inventory and retirement decisions; flag unavailable evidence.

5. **Business Logic & Abuse**
   - Check workflow bypass, replay attacks,
     race conditions, and cross-tenant access.
   - Review rate limits and resource exhaustion risks.

## Workflow

1. Identify the stack/versions, attack surface, and trust boundaries;
   compare implementation with architecture/API security decisions.
2. Trace untrusted inputs to sensitive operations.
3. Verify security controls and authorization paths.
4. Run available relevant security checks and tests.
   Verify allowed and denied user/role/object/tenant cases and absence of
   unauthorized effects; report missing cases as gaps with proposed tests.
5. Prioritize findings by impact and exploitability.
6. Recommend fixes and verification tests.

## Rules

- Select applicable OWASP API Security Top 10:2023 risks and ASVS 5.0.0
  requirement IDs; record exclusions without implying full compliance.
- Report only evidence-supported findings.
- Distinguish confirmed issues from potential risks.
- Do not treat missing evidence as proof of security.
- Trace the complete enforcement path; a repository method name or
  disabled CSRF alone is not proof of a vulnerability.
- Do not modify code unless explicitly requested.
- Do not run destructive or unauthorized security tests.

## Output

For each finding, report:
- Severity: Critical, High, Medium, or Low.
- Affected file and line.
- Vulnerability and realistic attack scenario.
- Evidence supporting the finding.
- Recommended remediation.
- Verification status.

Summarize applicable controls as verified, failed, or unverified with
enforcement locations and test commands/results. State design deviations,
review limitations, and untested areas; do not claim unverified checks passed.
