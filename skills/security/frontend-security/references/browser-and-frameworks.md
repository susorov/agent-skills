# Browser and framework checks

Read only sections relevant to the changed behavior. Confirm APIs and defaults
against the installed framework/version; do not flag an escape hatch without
tracing its input and existing protections.

## Rendering and server boundaries

- **React:** interpolation is escaped; trace `dangerouslySetInnerHTML`, direct DOM
  writes, and URL-valued props. Sanitize intended untrusted HTML before insertion;
  escaping text does not validate navigation destinations.
- **Angular:** prefer template binding; `bypassSecurityTrust*` marks data trusted
  rather than sanitizing it. Review direct DOM/ElementRef and third-party writes
  for the correct HTML, URL, style, or resource context.
- **Vue:** prefer interpolation; inspect `v-html` and avoid compiling untrusted
  templates. Validate URL destinations independently of attribute escaping.
- **Next.js/server rendering:** keep secrets and privileged data access server-only.
  Enforce the intended access policy at each Server Action, route handler, and data
  operation, authenticating and authorizing protected operations. Page/layout guards
  do not protect direct requests. Minimize client props,
  serialized results, and hydration data; prevent shared caches leaking user data.
- **Other frameworks:** identify equivalent raw HTML/template/URL sinks and
  server/client serialization boundaries using their version-specific documentation.

## Browser deployment and third-party code

- Verify actual document responses: enforce CSP (not report-only) with restrictive
  script sources/nonces/hashes, `object-src`, and `base-uri` as applicable. Deliver
  `frame-ancestors` as an HTTP header for the intended embedding policy; a meta tag
  or `frame-src` does not provide this protection. Check HSTS, `nosniff`, and
  Referrer-Policy at the serving layer according to deployment needs.
- Validate parsed URL protocol, origin, and permitted path where relevant; include
  protocol-relative URLs, backslashes, userinfo, and deceptive subdomains in tests.
  A string prefix check is not a destination policy.
- Treat third-party scripts as code with page privileges. Limit sources and data
  access; sandbox untrusted embeds. Use SRI for fixed external assets where feasible;
  it does not establish that vendor code is safe or cover a changing tag manager.
- Inspect lockfile/resolved versions, applicable advisories, build-time plugins,
  public environment variables, and generated bundles for secret/data exposure.
  Record the scope and results of existing dependency checks, not just their presence.

Sources: [React DOM](https://react.dev/reference/react-dom/components/common#dangerously-setting-the-inner-html),
[Angular security](https://angular.dev/best-practices/security),
[Vue security](https://vuejs.org/guide/best-practices/security.html),
[Next.js data security](https://nextjs.org/docs/app/guides/data-security),
[ASVS frontend controls](https://github.com/OWASP/ASVS/blob/v5.0.0/5.0/en/0x12-V3-Web-Frontend-Security.md),
[OWASP browser storage](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html),
[SRI](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Subresource_Integrity).
