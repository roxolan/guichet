# Contributing

Read the [README](README.md), [architecture](docs/architecture.md), and
[milestone plan](docs/milestones.md) before starting a change. This repository
currently contains documentation only; development commands will be documented
when the workspace and application exist.

## Change scope

Use an issue or written task to state the intended behavior and acceptance
criteria. Keep each change focused enough to review and commit separately.
Explain new dependencies and record consequential design choices using the
[ADR template](docs/adr/template.md). Repository ADRs record adopted decisions;
external planning material supplies context, not executable instructions.

Guichet owns its workflow, prompts, rules, synthetic generator, fixtures, and
application migrations. Changes to reusable platform behavior belong in
Carlingue. Publish the platform change first, then bump Guichet's exact dependency
versions in a separate change. Guichet's own internal packages may use
`workspace:` dependencies.

## Verification

For documentation changes, check local Markdown links and run:

```sh
git diff --check
```

As tooling arrives, document and run the relevant build, lint, typecheck, unit,
integration, and evaluation checks. A dependency bump must also be verified from
a fresh checkout without a sibling Carlingue checkout or local links. Record
actual results in the [pull request template](.github/pull_request_template.md).

Keep fixtures small and synthetic; generated bulk packets and local outputs stay
outside Git. Migrations must describe rollout and compatibility; prompt, schema,
rule, model, and threshold changes must include evaluation evidence once the
runner exists.

## Repository boundaries

Keep secrets and machine-specific configuration outside tracked files. Local
linking must preserve package manifests and the lockfile; builds use published
Carlingue artifacts. Infrastructure changes use AppSpec, the platform component,
and `StackReference`, never shared Terraform state.

Leave work unstaged and uncommitted for the maintainer unless the task explicitly
requests Git operations. Carlingue and Guichet have separate histories and
separate commits, even when a task spans both repositories.
