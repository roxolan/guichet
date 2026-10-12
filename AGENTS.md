# Agent working agreements

- Read the README, relevant architecture decisions, and the current milestone
  before editing. User instructions define the requested scope.
- Work from a written task with intent and acceptance criteria. Keep changes
  small and identify unresolved decisions instead of silently expanding scope.
- Current work is documentation only. Describe planned scripts, packages, and
  deployments as planned until they exist and have been verified.
- Guichet owns lending-domain behavior and synthetic data. Shared platform
  changes belong in Carlingue, followed by publication and a dependency bump.
- Consume external `@carlingue/*` packages at exact published versions. Do not
  commit `workspace:`, `link:`, `file:`, Git, or relative source dependencies on
  Carlingue. `workspace:` is allowed for Guichet's own workspace packages.
- Local linking is planned as a reversible development aid that leaves manifests
  and the lockfile unchanged. A sibling checkout may support that explicit local
  override, but committed dependencies, CI, and fresh-checkout builds must work
  without it or a filesystem symlink.
- Tenant infrastructure uses the published platform stack outputs through
  `StackReference`. Do not read Terraform state or provision shared infrastructure
  from this repository.
- Keep data synthetic and the README's decision boundary true. Do not add credit
  decisions, real applicant data, credentials, or private project context.
- Record consequential architecture changes with the ADR template. Keep docs
  synchronized with implemented behavior and distinguish targets from evidence.
- Run checks appropriate to the change and report what ran, what passed, and
  what could not be verified. Do not invent successful checks.
- Leave changes unstaged and uncommitted for the user. Do not stage, commit,
  push, publish, or provision resources unless explicitly requested.
