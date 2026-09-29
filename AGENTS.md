# AGENTS.md — layer-kind

Standalone candy repo for the `kind` layer — the pinned Kubernetes-in-Docker
tool and its per-engine host preconditions. The candy lives in `charly.yml` at
the repo root: the `KIND_VERSION` var, the `kind-engine-prepare` plan step, the
`check:` assertions, and the embedded `skill:` entity projected into the
marketplace corpus as `/charly-kubernetes:kind`.

Canonical files:

- `charly.yml` — the `kind:` candy entity and the `kind-skill:` skill entity.
- `CHANGELOG/` — per-CalVer release history; read it before changing baked checks.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-kubernetes:kind` — the owning skill. The `kind` CLI, the
  `kindcluster:` substrate, engine selection via `KIND_EXPERIMENTAL_PROVIDER`,
  the per-engine preconditions, and teardown. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`check:`, per-distro `distro:` arms,
  package/repo sections, and service declarations). Load before editing any
  entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.
- The pinned `KIND_VERSION` and the sha256-verified release manifest are the
  contract; a version bump must keep both the download step and its `check:`
  assertions aligned.

## Modify this repo

- Edit the `kind:` candy entity AND the `kind-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a version or
  behaviour change not mirrored in the skill leaves the corpus stale.
- `kind` is host tooling composed by a `target: local` deploy; the
  `kind-engine-prepare` script owns the per-engine preconditions — do not move
  that logic into a check.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
