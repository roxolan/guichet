# Architecture

Status: intended v1 architecture; documentation scaffold only.

## Responsibilities

Guichet contains the lending-intake domain and consumes Carlingue as an external
tenant. Its builds must work from its own checkout using published dependencies.
The [tenant ADR](adr/0001-consume-carlingue-as-an-external-tenant.md) records this
boundary.

| Owner | Responsibility |
| --- | --- |
| Guichet | Synthetic generator, intake workflow, rules, prompts, review UI, fixtures, migrations, tenant AppSpec and thin Pulumi program |
| Carlingue packages | Validated AppSpec and platform output schema, CI generator, deployment component, generic evaluation and runtime helpers as consumers require them |
| Carlingue Terraform | Shared AWS network, ECS cluster, shared ingress, registry, DNS/certificates, CI and observability infrastructure |
| Carlingue output bridge | Read platform-owned Terraform outputs and publish the versioned `carlingue-platform` Pulumi stack interface |
| Guichet Pulumi stacks | Read that interface through `StackReference` and create tenant/environment resources through the platform component |

Terraform and Pulumi must not both manage the same resource. Guichet never reads
Terraform state. Committed code must not import platform source directly across
repositories; temporary local package linking is the documented development
override.
Secret values do not belong in AppSpec, documentation, or the published output
contract; the interface carries references where needed.

## Application shape

The intended flow is packet intake, queued processing, completeness and
consistency checks, routing, and human review. The worker records explanations,
provenance, model/prompt versions, and audit events. Low confidence, budget limits,
and failures must have explicit routes to review or bounded recovery.

The planned service roles are `intake-api`, `worker`, `review-ui`, and a
deterministic `mock-verification-api`. A synthetic-check job follows when there
is a deployed environment. Lending rules and scenario generation are tenant
code; generic orchestration and delivery mechanisms are platform code.

## Planned repository shape

The following paths are destinations for later implementation, not existing
packages or commands:

| Path | Purpose | First needed |
| --- | --- | --- |
| `app.spec.ts` | Tenant identity, service declarations, hooks and resource intent | M0 |
| `services/` | Initial sample, then intake, worker, UI and verification services | M0 / M2 |
| `packages/synthetic-lending/` | Seeded scenarios, rendered documents, faults and ground truth | M1 |
| `packages/domain/`, `packages/rules/` | Domain types, prompts and deterministic checks | M2 |
| `eval/` | Small synthetic fixture subset and reviewed evaluation configuration | M1 onward |
| `migrations/` | Application database changes | M2 |
| `.buildkite/` | Entry point invoking the installed platform CI generator | M0 |
| `infra/` | Thin tenant Pulumi program and environment conventions | M4 |
| `scripts/` | Reversible local development helpers | M0 onward |

## Local and cloud execution

Local Docker Compose will start Guichet and its required backing services. Add
services as their consumers arrive. The generator supplies fixtures and ground
truth before the agent workflow is implemented.

The selected v1 cloud substrate is AWS Fargate in a personal account. Carlingue
will provision shared infrastructure in M3; Guichet's per-PR and demo environments
follow in M4. A platform deployment must work for Carlingue's `hello-tenant` before
it is adopted here. M0-M2 local work requires no AWS credentials.

Database/queue hosting, region, domain, state backends, and account access remain
implementation decisions. Guichet inherits published platform interfaces for
these concerns rather than selecting an independent infrastructure path.

## Dependency contract

Pin every external `@carlingue/*` dependency to an exact published version and
commit the lockfile. Snapshots enable iteration before stable releases; dependency
bump changes provide a reviewable record of adoption. Use the published base
image when available, pinned to an immutable reference.

A planned `dev:link` helper may replace installed packages locally while leaving
manifests and the lockfile unchanged. It must be reversible. `workspace:` remains
valid between Guichet's own packages. Local links and sibling symlinks do not
change the production dependency contract.
