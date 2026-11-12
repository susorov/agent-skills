# Spring Boot security checks

Use for Spring Boot implementation or review. Apply checks to the changed behavior
and its enforcement path; mark unrelated features not applicable. Match official
documentation to the detected versions rather than copying configuration blindly.

- **Establish context:** inspect Maven/Gradle resolved dependencies, Boot/Security
  versions, MVC versus WebFlux, active deployment profiles, and authentication mode.
  Check relevant advisories against actual versions and configuration.
- **Request security:** inspect every security chain's matcher, order, authorization
  rules, and fallback coverage. Verify public routes are intentional and protected
  routes cannot miss all chains. Custom chains change Boot's security
  auto-configuration; explicitly verify Actuator exposure and access rules,
  including a separate management port when configured.
- **Service authorization:** when using method annotations, verify the appropriate
  method-security configuration is enabled and calls traverse the enforcing proxy.
  Check self-invocation and alternate callers. Verify object and tenant permissions
  before access or mutation; roles alone do not establish ownership.
- **JWT resource servers:** verify the decoder uses trusted issuer/key sources,
  permitted algorithms, signature, audience, token-purpose, expiry/not-before
  validation, and deliberate authority mapping. Do not accept decoded claims as
  verified identity or replace existing validators accidentally.
- **DTOs:** use explicit writable fields and mapping; derive or validate ownership
  and tenant membership server-side. Verify Bean Validation actually runs, including
  nested input. Validation does not grant field permissions. Restrict response fields.
- **Persistence:** bind JPQL/native query values and allowlist dynamic identifiers
  or sort choices. Trace tenant/object constraints through reads, lists, bulk writes,
  caches, and jobs in scope. Protect mutation invariants with appropriate database
  constraints, transactions, locking, or version checks across instances; test
  concurrent requests and replay where relevant.
- **Browser access:** configure credential-compatible CORS origins and test
  preflights. Preserve CSRF defenses for automatically attached browser credentials,
  including cookies and browser-managed Basic authentication. Statelessness alone
  does not justify disabling CSRF.
- **Production behavior:** inspect profile overrides, secret sources/access,
  sanitized error responses, and safe authentication/authorization audit events.
  Test sensitive-data redaction without suppressing useful security evidence.
- **Verification:** use JUnit and Spring Security Test with relevant filters and
  service proxies active. Exercise allowed access, anonymous/insufficient-role
  denial, other-owner/tenant denial, and prohibited fields; assert no unauthorized
  data or state change. Mock `jwt()` tests cover authorization, not decoder
  validation: separately test real `JwtDecoder`/`ReactiveJwtDecoder` rejection of
  invalid signatures and claims. Record commands, results, and untested controls.

Official references: [filter chains](https://docs.spring.io/spring-security/reference/servlet/configuration/java.html),
[method security](https://docs.spring.io/spring-security/reference/servlet/authorization/method-security.html),
[JWT](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html),
[Actuator](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html),
[CSRF](https://docs.spring.io/spring-security/reference/features/exploits/csrf.html),
[testing](https://docs.spring.io/spring-security/reference/servlet/test/mockmvc/oauth2.html).
