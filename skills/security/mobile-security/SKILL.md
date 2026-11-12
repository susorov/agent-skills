---
name: mobile-security
description: >
  Design, implement, and review Android/iOS application security, including
  native, Flutter, and React Native apps. Use for changes to authentication,
  storage, networking, permissions, platform integration, SDKs, or release
  configuration; not language-only matches or purely cosmetic/documentation edits.
---

# Mobile Security

Build mobile applications with security enforced by default.

For storage, networking, platform integration, or release configuration, read the
applicable platform sections of [platforms.md](references/platforms.md).

## Requirements

1. **Data Storage & Secrets**
   - Encrypt stored Android credentials with Keystore-protected keys; Keystore
     stores keys, not arbitrary tokens. Use iOS Keychain with explicit access policy.
     Verify wrapper backing, backup/sync behavior, and cleanup on logout/account changes.
   - Never hardcode secrets or private keys.
   - Minimize leaks through logs, backups, caches, screenshots, and clipboard;
     apply available platform controls and document remaining capture limitations.
   - Minimize collection/retention, disclose purposes and SDK sharing, and provide
     appropriate consent and deletion controls, including required server support.

2. **Authentication & Authorization**
   - Use secure authentication and session management.
   - For native OAuth2/OIDC, use authorization code with PKCE S256 through an
     external user agent; validate registered redirects and bind callbacks to
     the initiating session with state/nonce as the flow requires.
   - Define expiry, refresh, and logout behavior; remove local credentials and
     coordinate server revocation. Local deletion alone does not invalidate a session.
   - Enforce server-side authorization for remote resources.
   - Use platform-supported biometric authentication
     for appropriate local security controls.

3. **Network Security**
   - Require HTTPS with proper TLS validation.
   - Never disable certificate or hostname verification.
   - Restrict cleartext traffic and insecure endpoints.
   - Validate untrusted network responses.
   - Apply certificate pinning only when justified
     by the threat model and operational requirements.

4. **Platform Security**
   - Validate link/IPC destinations, caller identity where available, and inputs;
     require authorization before sensitive actions, even for verified app links.
   - Minimize exported components and permissions; enforce access at each entry point.
   - Restrict WebView navigation/origins and bridge capabilities; never expose
     privileged native bridges to untrusted pages or frames.
   - Apply Android/iOS-specific security protections.

5. **Code & Application Integrity**
   - Validate untrusted input and prevent injection.
   - Use maintained platform cryptography and cryptographically secure randomness.
   - Check resolved SDK/dependency versions against applicable advisories and
     review their permissions, native behavior, and data collection.
   - Disable debugging and development configurations
     in production builds; verify the release artifact's effective configuration.
   - Consider tampering and reverse-engineering
     protections according to application risk.

## Workflow

1. Identify the requested mode, platform/framework versions, and sensitive assets.
2. Map entry points, data flows, and trust boundaries.
3. Design: record controls, client/server enforcement, and acceptance tests.
   Implement: apply controls within scope. Review: inspect without editing unless asked;
   verify full flows and existing protections before reporting vulnerabilities.
4. Select MASVS 2.1.0 control IDs and document applicability/exclusions; selecting
   NETWORK-2 requires pinning evidence. A risk-based omission is not a passed control.
5. During implementation, add/update and run relevant positive and negative tests:
   token cleanup, forged callbacks/links, unauthorized IPC/bridges, invalid TLS,
   and release configuration as applicable. During review, run authorized checks.
6. Report severity, file/line evidence, impact, minimal fixes, enforcement locations,
   commands/results, unresolved risks, and unverified controls or backend dependencies.

## Rules

- Never trust the mobile client as a security boundary
  for remote backend authorization.
- Prefer native platform security APIs.
- Never disable security protections for convenience.
- Never embed server-side secrets in mobile applications.
- Apply security controls proportionally to risk.
- Do not claim security checks passed without verification.
