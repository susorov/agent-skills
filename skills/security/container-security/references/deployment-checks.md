# Docker Compose and Spring Boot deployment checks

Read only applicable sections. Match options to the installed versions and target
platform; source configuration does not establish runtime enforcement.

## Effective Compose configuration

- Establish the actual deployment command, ordered `-f` files, includes/extends,
  profiles, environment sources, and image/build target. Inspect the merged model
  with `docker compose config`; use `--quiet` for syntax validation. Avoid printing
  resolved credentials or interpolation environments; redact evidence before sharing.
- Verify the resulting ports and mounts after every override. Adding a loopback
  port mapping can retain an earlier unrestricted mapping; replace/remove the
  original using syntax supported by the installed Compose version, then recheck.
- `EXPOSE`/`expose` do not publish host ports or restrict same-network access.
  Inspect host IP bindings and service membership; use internal networks where
  egress is unnecessary. Preserve required outbound access and service authentication.
  Host networking changes isolation; actual reachability also depends on the host.
- Inspect socket mounts and `use_api_socket`, privileged hooks, host PID/IPC/network
  or user namespaces, device grants, and unconfined security options where supported.
  Rootless engines and non-root processes are different controls; neither makes
  broad host access harmless. Do not infer root solely from a missing `USER` override.
- Grant each service only its required secrets. Protect file-backed secret sources;
  do not assume Compose provides encrypted storage or enforces requested uid/gid/mode
  for bind-mounted secret files. Verify the running UID can read the intended file.
  `_FILE` support is image/application-specific, not a universal Compose mechanism.
- Check effective CPU/memory/PID limits and logging policy for the selected runtime;
  do not assume all `deploy` attributes are supported, or that all are ignored.
  The default `json-file` driver needs explicit rotation unless a bounded host policy
  applies. Keep required writable data/temp paths narrow and correctly owned.

## Spring Boot workloads

- Inspect resolved Boot and Java versions, build output, active profiles, and runtime
  overrides. Match build/runtime Java compatibility; prefer a maintained minimal runtime
  image, retaining tools only when needed. Multi-stage builds do not by themselves
  remove secrets from builder caches or generated artifacts. Layer extraction is
  optional and its commands depend on the Boot version.
- Verify effective UID, JAR/config readability, database-volume ownership, and required
  temp/upload/log paths before enforcing non-root or read-only settings. Use bounded
  writable mounts where needed; preserve database initialization and persistent writes.
- Leave container memory headroom for non-heap/native JVM use; do not set heap equal
  to the container limit or prescribe a universal percentage. Use exec-form startup
  or an equivalent signal-forwarding wrapper and verify graceful shutdown when authorized.
- Trace mounted secret files into actual properties. Use supported `configtree:`
  imports with names matching the required properties, or another verified integration;
  a `SPRING_DATASOURCE_PASSWORD_FILE` variable alone does not establish file ingestion.
- Inspect JDWP/JMX flags, management ports, Actuator exposure, and profile overrides.
  Keep production debug access disabled unless explicitly justified; verify required
  management access and application authorization separately from container networking.

Sources: [Compose configuration](https://docs.docker.com/reference/cli/docker/compose/config/),
[merge behavior](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/),
[service attributes](https://docs.docker.com/reference/compose-file/services/),
[Compose secrets](https://docs.docker.com/compose/how-tos/use-secrets/),
[build secrets](https://docs.docker.com/build/building/secrets/),
[image maintenance](https://docs.docker.com/build/building/best-practices/),
[logging](https://docs.docker.com/engine/logging/configure/),
[Spring Dockerfiles](https://docs.spring.io/spring-boot/reference/packaging/container-images/dockerfiles.html),
[Spring configuration](https://docs.spring.io/spring-boot/reference/features/external-config.html),
[OWASP Docker](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html),
[NIST SP 800-190](https://csrc.nist.gov/pubs/sp/800/190/final),
[CIS Docker](https://www.cisecurity.org/benchmark/docker).
