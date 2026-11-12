---
name: frontend-security
description: >
  Design, implement, and review browser application security, including
  React, Angular, Vue, and Next.js. Use for changes to authentication,
  untrusted rendering, API integration, routing, storage, security
  configuration, or third-party code; not purely cosmetic or documentation edits.
---

# Frontend Security

Build frontend applications with security enforced by default.

For rendering, framework boundaries, browser configuration, or third-party code,
read the applicable sections of [browser-and-frameworks.md](references/browser-and-frameworks.md).

## Requirements

1. **XSS & Injection**
   - Treat all external and user-controlled data as untrusted.
   - Prevent DOM-based, reflected, and stored XSS.
   - Avoid unsafe HTML rendering, eval, and dynamic execution.
   - Prefer text rendering and contextual encoding; sanitize deliberately
     supported HTML with a maintained sanitizer before reaching an HTML sink.

2. **Authentication & Authorization**
   - Define token/session expiry, refresh, logout cleanup, and server invalidation.
     For browser OAuth/OIDC, use authorization code with PKCE; embed no client secret.
   - Prefer server-managed HttpOnly cookie sessions where the architecture permits.
     Avoid authentication tokens in localStorage, sessionStorage, or IndexedDB;
     in-memory tokens reduce persistence but remain exposed to executing scripts.
   - Have the server set Secure, HttpOnly, suitable SameSite, and narrow cookie scope.
     For browser-attached credentials, verify server-side CSRF token/origin checks;
     SameSite is defense in depth. Coordinate server changes within task scope.
   - Never rely on frontend authorization for data protection.

3. **Browser & Data Security**
   - Verify effective response headers, including CSP and clickjacking protection,
     at the serving application/CDN; frontend source alone does not prove delivery.
   - Minimize sensitive browser storage and clear user-specific caches on logout.
   - Prevent sensitive data exposure in URLs, logs, and errors.
   - Use HTTPS and secure cookie attributes where applicable.

4. **API & Third-Party Security**
   - Validate assumptions about API responses.
   - Avoid embedding secrets in frontend bundles.
   - Parse redirects and external URLs against a trusted base; allow intended
     schemes and exact destinations, rejecting userinfo and deceptive host matches.
   - Review third-party script privileges and resolved dependency versions;
     check applicable advisories and production build exposure.
   - Configure CORS on the server; never treat it as authorization.

5. **Framework Security**
   - Detect the frontend framework and version.
   - Apply framework-specific XSS and rendering protections.
   - Review unsafe DOM APIs, hydration, and template handling.
   - Avoid bypassing built-in security mechanisms.

## Workflow

1. Identify the requested mode, framework/version, entry points, and trust boundaries.
2. Trace untrusted data to rendering and sensitive operations.
3. Design: record security decisions, client/server enforcement, and acceptance tests.
   Implement: apply controls within scope. Review: inspect without editing unless asked;
   confirm findings across the full data flow and account for existing protections.
4. Review authentication, storage, and API integration; identify required backend
   changes separately when outside scope rather than claiming client code enforces them.
5. During implementation, add/update and run relevant positive and negative tests:
   malicious HTML/URLs, unauthorized actions, CSRF, and session cleanup as applicable.
   During review, run available relevant checks when authorized.
6. Report findings with severity, file/line evidence, impact, and minimal fixes;
   record enforcement locations, commands/results, and unresolved or unverified controls.

## Rules

- Never trust client-side validation or authorization alone.
- Prefer framework-provided safe rendering mechanisms.
- Never expose secrets in client-side code.
- Do not disable security protections for convenience.
- Use selected ASVS 5.0.0 requirements and OWASP Top 10:2025 as a risk guide;
  record applicable control IDs and justified exclusions for the reviewed scope.
- Do not claim security checks passed without verification.
