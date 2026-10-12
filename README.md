# Guichet

Guichet is a public tenant of [Carlingue](https://github.com/roxolan/carlingue).
It will demonstrate a lending-intake workflow using published, pinned Carlingue
packages and the platform's infrastructure interface. Domain logic belongs here;
shared delivery and runtime capabilities belong in Carlingue.

## Current status

**M0 documentation scaffold only.** The repository records scope, architecture,
and contribution agreements. There is no application, package manifest, runnable
command, CI pipeline, or deployed environment yet. Toolchain setup and consumption
of the first published Carlingue snapshot are subsequent M0 work.

## Intended behavior

Guichet will accept synthetic application packets, classify and extract documents,
check completeness and cross-document consistency, and route the packet to
completion, a document request, or human review. Reviewer corrections will feed
an evaluation dataset; processing decisions will retain provenance and audit
history.

The initial scope is one workflow: a manifest, four document types, six to eight
checklist rules, a small review and operations UI, and one evaluation suite.
The synthetic generator comes before the agent workflow.

## Data and decision boundary

Guichet is a platform-engineering demonstration, not a lending product. It has no
users or real customer data. All future fixtures and demo packets must be
synthetic, with invented people, employers, institutions, and branding;
identifiers must be invalid by design and addresses non-resolving.

It checks completeness and consistency and routes work. It does not score
applicants, price loans, underwrite, or make credit decisions. Completing or
approving a packet means completing intake, never approving a loan.

## Development and deployment direction

Development will run locally with Docker Compose before cloud deployment.
The v1 deployment target is AWS ECS on Fargate in a personal AWS account.
Carlingue owns the shared infrastructure; Guichet will declare its services in
AppSpec and use a thin Pulumi program for tenant environments. M0 requires no AWS
connection. Account-backed infrastructure work begins in M3.

See [architecture](docs/architecture.md), the [milestone plan](docs/milestones.md),
and the [external-tenant decision](docs/adr/0001-consume-carlingue-as-an-external-tenant.md).
For changes, read [CONTRIBUTING.md](CONTRIBUTING.md) and [AGENTS.md](AGENTS.md).

Licensed under [Apache-2.0](LICENSE).
