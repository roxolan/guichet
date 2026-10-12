# Milestones

The shared [Carlingue milestone plan](https://github.com/roxolan/carlingue/blob/main/docs/milestones.md)
defines platform delivery. This file records Guichet's part of that work.

## M0 — Establish an independent platform consumer

Status: in progress; only the documentation foundation exists.

Intent: prepare Guichet's repository and then prove it can build and run against
published Carlingue artifacts without a platform checkout.

### Documentation batch

- [x] Describe purpose, synthetic-data boundaries, and documentation-only status.
- [x] Record platform ownership and the external-tenant dependency contract.
- [x] Add contribution and agent agreements, PR and ADR templates, and this task.
- [x] Record AWS Fargate in a personal account as the selected v1 destination.

This batch changes README, contributor/agent instructions, the PR template, and
`docs/`. It provisions nothing and introduces no application or toolchain code.

### Remaining acceptance criteria

- [ ] Bootstrap the pinned TypeScript/pnpm workspace and its checks.
- [ ] Reconcile this existing repository with Carlingue's tested tenant template;
      preserve reviewed documentation and local work rather than recreating it.
- [ ] Add a validated Guichet AppSpec and a runnable minimal service with health
      and smoke checks and a local Compose environment.
- [ ] Install exact versions from Carlingue's first real published snapshot and
      commit the lockfile; reject local/path/Git dependencies on Carlingue.
- [ ] Invoke the installed CI-generator CLI from Guichet's own Buildkite pipeline
      and obtain a successful build for a known commit.
- [ ] Provide and verify reversible local linking without manifest/lockfile edits.
- [ ] Verify install, build, lint, typecheck, tests, and local smoke checks from a
      clean checkout without Carlingue present.
- [ ] Adopt the first stable Carlingue release in a separate dependency-bump
      change and record the validation results.

M0 depends on Carlingue's AppSpec and output contract, runnable sample, CI
generator, tested tenant template, and working snapshot/stable publication.
Guichet's documentation can precede those artifacts. Do not insert fictional
package versions or claim the template generated this repository before it has
been exercised. No AWS connection is needed for M0; its Buildkite bootstrap uses
an available trusted agent.

### Evidence to retain

Link published package versions, source commits, the successful Guichet build,
and clean-checkout verification when they exist. The stable dependency bump
closes the initial release-and-consumption cycle. No such evidence exists yet.

## After M0

| Milestone | Guichet outcome | Dependency |
| --- | --- | --- |
| M1 | Seeded synthetic generator, fault catalogue, two document variants, committed fixtures and evaluation metrics | Generator work can start independently; completed M1 consumes the published Carlingue evaluation runner |
| M2 | Intake, bounded worker, rules, review/operations UI, mock verification, local tracing and end-to-end checks | M1 fixtures and published runtime helpers |
| M3 | Platform infrastructure becomes available; no shared Terraform is added here | Carlingue M0 stable, AWS account connection |
| M4 | Per-PR and demo environments, migrate/seed/smoke hooks, preview reporting and teardown | Runnable M2 Guichet, Carlingue shared infrastructure, and successful sample deployment |
| M5 | Evaluation gates and nightly trends, with a visible blocked regression | M1/M2 evaluation work and delivery pipeline |
| M6 | Measured SLOs, alerts, restore drill, incident exercise and operational evidence | Deployed tenant and platform reliability mechanisms |
| M7 | Accurate architecture, boundary, operational and handoff documentation | Evidence from the preceding work |

M3 may proceed alongside M1/M2 once M0 is stable; local application work does not
depend on AWS. Corbeille is deferred until the shared v1 finish criteria are met.

Open implementation choices include runtime/tooling versions, the exact first
platform versions, application auth/UI libraries, model/provider versions, and
tenant evaluation thresholds. Resolve each when its milestone needs it and
record consequential decisions in an ADR.
