# HardBreacher

Defensive research notes for an experimental Windows endpoint-security privilege-boundary proof of concept by [MSNightmare](https://github.com/MSNightmare).

## Status and scope

The original author reports that the proof of concept was tested against Kaspersky Endpoint Security for Windows 14.0.0.504 on Windows 11 25H2. The repository describes inconsistent execution and a successful run as creating `C:\Windows\System32\MY_SNAKE_IS_SOLID.dll` with permissions granted to the current user.

Those statements are repository claims, not independent verification. This fork does not assert a zero-day, vendor impact, CVE assignment, or reproducible exploit until the behavior is confirmed in an isolated lab and compared with current vendor guidance. Product names identify the reported test environment and do not imply that every version or configuration is affected.

## What the repository contains

- `HardBreacher/` contains the primary Windows C++ proof-of-concept project and a bundled SolidSnake DLL.
- `SolidSnake/` contains the companion DLL project.
- The repository includes build artifacts and a screenshot from the author's reported test.
- The current implementation is explicitly described by its author as unstable.

The code remains available for qualified defensive review. This fork will not improve reliability, stealth, persistence, evasion, one-click execution, or deployment against systems that are not owned by the researcher.

## Defensive research question

The useful question is whether a lower-privileged local process can influence an endpoint-security component in a way that crosses an intended trust boundary. A defensible result must identify:

1. The exact operating-system and endpoint-product versions, update state, policy configuration, and architecture.
2. The initial user's privileges and the integrity level of every relevant process.
3. The security descriptor before and after any reported file, registry, service, or object change.
4. The endpoint product component that performed the privileged action.
5. A repeatable control test showing that the same outcome does not occur without the suspected interaction.
6. Complete timestamps and hashes for the proof-of-concept binary, companion DLL, logs, and resulting artifacts.
7. Reproduction after a clean snapshot restore, not repeated execution on an already modified system.

Without that evidence, the repository should be treated as an interesting lead rather than a confirmed vulnerability.

## Safe validation model

Validation belongs in an isolated virtual machine with snapshots, no production credentials, no access to corporate networks, and no sensitive data. Use a dedicated test license where required. Preserve the original binaries and record hashes before testing. Capture operating-system event logs, endpoint-product logs, process ancestry, file and registry activity, token and integrity information, and the before/after access-control state.

A failed run is still evidence. Record the failure mode instead of repeatedly changing timing or execution behavior until it succeeds. Any attempt to stabilize the behavior should wait for vendor coordination and a written research plan that limits work to reproducibility in the isolated environment.

Do not run this repository on third-party endpoints, employee devices, customer systems, shared infrastructure, or internet-exposed hosts.

## Detection opportunities

Defenders evaluating the reported behavior can monitor for:

- Unexpected creation or permission changes for DLLs under `C:\Windows\System32`, including the filename reported by the author.
- Access-control changes that grant a standard user write, modify, or full-control rights in protected operating-system paths.
- Unusual process relationships involving endpoint-security UI components and lower-integrity user processes.
- Endpoint-product tamper, self-protection, service restart, or component-health events near the same timestamp.
- Security-service degradation followed by file-system or registry changes from an unexpected process.
- Repeated failed executions followed by a single privileged file-system change.
- New unsigned or unexpected modules loaded by endpoint-security processes.

These are investigation leads, not proof of exploitation. Baseline the product's normal updater, repair, and quarantine behavior to avoid mistaking legitimate maintenance for an attack.

## Evidence package for vendor review

A responsible report should include:

- Exact product build, signature/database version, operating-system build, and applied updates.
- A minimal reproduction description that omits unnecessary payload behavior.
- A clean snapshot reproduction rate and a matched negative control.
- Process, file-system, registry, token, and access-control evidence.
- Hashes of all submitted files and a clear statement of what each file does.
- The earliest and latest observed timestamps in UTC.
- Crash dumps or product diagnostics when available.
- A proposed impact statement separated from what was actually observed.
- A request for a vendor case identifier, remediation guidance, and coordinated disclosure timeline.

Do not publish operational details that increase exploitation reliability before the vendor has had a reasonable opportunity to investigate and remediate.

## Research conclusions that are not yet established

This repository alone does not establish that:

- The behavior affects current Kaspersky releases.
- The behavior is exploitable from a default configuration.
- The outcome is reliable across reboots or hosts.
- The reported file permission change produces arbitrary SYSTEM-level code execution.
- Endpoint protection can be persistently disabled.
- A CVE or vendor advisory exists.

Each conclusion requires separate evidence.

## Attribution

The original concept, code, screenshot, and reported test observations belong to [MSNightmare/HardBreacher](https://github.com/MSNightmare/HardBreacher). This Personfu fork adds defensive framing, validation requirements, detection guidance, and responsible-disclosure boundaries. It does not claim authorship of the original research.

## Legal and ethical use

Use only on systems you own or are explicitly authorized to test. Follow product license terms, local law, organizational policy, and coordinated-disclosure expectations. The maintainers do not authorize use for unauthorized access, security-control disruption, persistence, evasion, or harm.
