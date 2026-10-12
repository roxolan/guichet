# ADR 0001: Consume Carlingue as an external tenant

- Status: Accepted
- Date: 2026-10-11
- Implementation: Documentation only; consumer setup remains in M0.

## Context

Guichet demonstrates a lending-intake workflow and the ability of an independent
application to consume Carlingue. Keeping the domain in its own public repository
makes platform compatibility and release adoption observable. AWS Fargate in a
personal account is the selected v1 deployment target.

## Decision

Guichet owns its application, lending rules, prompts, synthetic data, fixtures,
and evaluation configuration. It consumes Carlingue through three interfaces:

1. Published `@carlingue/*` packages at exact versions.
2. Versioned `carlingue-platform` stack outputs through Pulumi `StackReference`.
3. A published runtime base image, pinned to an immutable reference when available.

Carlingue owns shared Terraform infrastructure and the bridge that publishes its
outputs. Guichet uses AppSpec and the platform deployment component in a thin
Pulumi program; it never reads Terraform state or manages shared resources.

Dependencies on platform source via relative paths, `workspace:`, `link:`,
`file:`, or Git URLs are excluded from committed configuration. Guichet's own
workspace packages may use `workspace:`. A future reversible local linking helper
must preserve manifests and the lockfile. Sibling repository symlinks are only
local navigation aids.

M0 starts with this existing documentation scaffold. Once the tenant generator
exists, reconcile and validate its output here without blindly overwriting the
repository. Cloud deployment follows platform infrastructure work in M3/M4.

## Consequences

Platform changes require a release and a separate Guichet dependency bump. This
adds a publication dependency to M0, while making fresh-checkout compatibility
and version adoption testable. Missing platform features must be developed in
Carlingue rather than copied here. Local-first development remains independent
of AWS account access.

## Alternatives considered

- An in-tree tenant is simpler to iterate on, but does not prove independent
  consumption; Carlingue plans `hello-tenant` for in-tree testing.
- Source/path dependencies avoid publishing, but couple builds to checkout layout
  and conceal missing release artifacts.
- Direct Terraform-state access couples a tenant to platform internals and gives
  it a broader interface than the published output contract.

## Verification

M0 will install real snapshots and then stable packages in a clean Guichet
checkout, run its checks without Carlingue present, and record the dependency
bumps. M4 will deploy through the published stack interface after the platform
sample succeeds. These checks have not run yet.
