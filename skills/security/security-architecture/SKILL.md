---
name: security-architecture
description: >
  Design secure backend and API architectures.
  Use when defining or changing service topology,
  trust boundaries, infrastructure exposure,
  or sensitive data flows.
---

# Security Architecture

Design systems with security enforced by default.

## Requirements

1. **Threat Modeling**
   - Identify assets, actors, entry points, and threats.
   - Map data flows and trust boundaries.
   - Consider spoofing, tampering, data exposure,
     privilege escalation, and denial of service.

2. **Trust & Access**
   - Apply zero-trust and least-privilege principles.
   - Deny access by default.
   - Enforce authentication and authorization at
     every relevant trust boundary.
   - Isolate users, tenants, and services.

3. **Data Protection**
   - Classify sensitive data.
   - Encrypt sensitive data in transit and at rest
     according to risk and applicable requirements.
   - Minimize data collection and retention.
   - Define secret/key storage, access, rotation,
     revocation, and recovery responsibilities.

4. **Infrastructure & Dependencies**
   - Minimize publicly exposed components.
   - Segment networks and restrict service access.
   - Use least-privilege identities and permissions.
   - Inventory API hosts, versions, environments, owners,
     external dependencies, and retirement plans.
   - Define trusted dependency sources and risk-based
     update/remediation deadlines for the dependency inventory.

5. **Resilience & Monitoring**
   - Design for rate limits, timeouts, and
     resource exhaustion protection.
   - Define security logging, monitoring, and alerts.
   - Avoid logging secrets or sensitive data.
   - Define fail-closed security behavior and bounded
     recovery when dependencies fail.

## Workflow

1. Understand the system and security requirements.
2. Identify assets, threats, and trust boundaries.
3. Evaluate architectural attack paths.
4. Select secure defaults and mitigating controls.
5. Document assumptions and residual risks.
6. Map high-risk threats to implementation requirements,
   enforcing components/owners, and verification methods.

## Rules

- Prefer established security patterns and standards.
- Never trust external input or network location alone.
- Avoid unnecessary components and attack surfaces.
- Never rely solely on perimeter security.
- Flag unresolved critical risks before implementation.
- Select applicable OWASP ASVS 5.0.0 requirements and record
  their IDs and exclusions; do not imply full compliance.

## Output

For each architectural design, provide:
- Key components and trust boundaries.
- High-risk threats and attack paths.
- Required security controls.
- Security assumptions and tradeoffs.
- Residual risks and validation requirements.
