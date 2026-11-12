---
name: container-security
description: >
  Design, implement, and review Dockerfile, Docker Compose, and associated
  container runtime security. Use for image/build, privilege, networking,
  volume, secret, or runtime configuration changes and security reviews;
  not Kubernetes/OpenShift cluster administration or cosmetic/documentation edits.
---

# Container Security

Build and run containers securely by default.

For Compose deployments or Spring Boot workloads, read the applicable sections of
[deployment checks](references/deployment-checks.md).

## Requirements

1. **Images & Builds**
   - Use trusted, minimal, maintained base images.
   - Prefer immutable production image digests from trusted sources; record
     the target platform and refresh pins through tested rebuilds.
   - Prefer multi-stage builds to reduce attack surface.
   - Never embed secrets in images, layers, or build arguments.
     Use BuildKit secret/SSH mounts; do not copy or print their contents.
     Inspect build contexts, .dockerignore, COPY inputs, outputs, and caches;
     deleting a secret in a later layer does not remove earlier copies.
   - Scan the final image and application dependencies with available tooling;
     record digest/platform, scan date/results, remediation, and justified exceptions.

2. **Runtime Security**
   - Verify the effective runtime UID, including image defaults and overrides;
     prefer non-root execution compatible with initialization and storage ownership.
   - Avoid privileged mode; prefer dropping all capabilities and adding only
     demonstrated requirements, with no-new-privileges and effective confinement.
   - Inspect Docker socket/API delegation, host namespaces, devices, and sensitive
     host mounts. Require explicit justification for elevated access; a read-only
     socket mount does not make Docker API operations read-only.
   - Retain applicable seccomp and AppArmor/SELinux protections; flag unconfined settings.
   - Prefer read-only root filesystems with narrow writable volumes/tmpfs for
     demonstrated needs; verify permissions rather than granting broad write access.

3. **Networking & Exposure**
   - Publish only required services on intended host interfaces; keep databases,
     caches, and management/debug ports private unless explicitly required.
   - Distinguish EXPOSE/expose from host publication; inspect network membership,
     ingress and egress needs. Network placement does not replace service authentication.
   - Never expose Docker daemon access unnecessarily.

4. **Secrets & Storage**
   - Grant runtime secrets only to required services; protect host sources and
     verify application ingestion. File-backed Compose secrets are not encrypted storage.
   - Never commit credentials or sensitive environment files.
   - Restrict mount sources, targets, access modes, and ownership; preserve required
     database writes while avoiding sensitive host directories and cross-service access.

5. **Configuration & Maintenance**
   - Inspect effective configuration, not just individual source files.
   - Apply workload-compatible CPU, memory, and PID limits; verify runtime support.
   - Bound log retention and avoid logging secrets or sensitive payloads.
   - Remove insecure defaults and unused privileges.
   - Rebuild, rescan, and test updated images/dependencies before authorized rollout.
   - Apply relevant Docker security best practices.

## Workflow

1. Identify the requested mode, target OS, Engine/Compose versions, deployment mode,
   build context/target, deployment command, and relevant trust boundaries.
2. Resolve selected files, overrides, profiles, and interpolation sources; inspect
   effective images, identity, mounts, privileges, networks, and ports without leaking secrets.
3. Design: record controls and acceptance checks. Implement: change requested files.
   Review: inspect without editing unless asked; trace actual enforcement and existing controls.
4. Verify workload compatibility, including required writes, ownership, and connectivity.
5. Run available configuration/lint and image checks within scope; when execution is
   authorized, test startup, required access, and denied writes/connections as applicable.
   Syntax validation alone does not establish runtime enforcement or security.
6. Report severity, file/line evidence, realistic impact, minimal fixes, commands/results,
   and unresolved or unverified controls; do not infer Internet reachability from a port alone.

## Rules

- Follow least-privilege and defense-in-depth principles.
- Never weaken isolation for convenience.
- Do not break required application functionality.
- Keep cluster administration outside scope; report host/daemon patching and security
  dependencies without changing them unless requested. Match controls to the target platform.
- Do not modify running or production containers
  without explicit approval.
- Do not run destructive or unauthorized commands.
- Do not claim security checks passed without verification.
