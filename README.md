# layer-kind

The `kind` ([Kubernetes-in-Docker](https://kind.sigs.k8s.io/)) tool plus its
per-engine host preconditions, as a standalone OpenCharly layer repo.

`kind` provisions local Kubernetes clusters as **node containers** on the
operator's container engine. It is host tooling, so the candy is composed by a
`target: local` deploy on the operator host (or baked into a box that will run
kind). The engine is charly's own engine word (`podman` / `docker` / `nerdctl`),
mapped 1:1 to kind's `KIND_EXPERIMENTAL_PROVIDER`.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `kind` |
| Pinned version | `v0.33.0` (the `KIND_VERSION` var) |
| Binaries | `/usr/local/bin/kind`; `kubectl` / `helm` from the required `layer-kubernetes` candy |
| Helper | `kind-engine-prepare` — applies the per-engine host preconditions |
| Dependencies | `layer-kubernetes` (kubectl + helm) |
| Service / port | none (host tooling) |

On Arch the distro `kind` package is used; elsewhere the pinned release binary is
installed sha256-verified against the upstream release manifest.

## How to use it

Compose the layer as a nested `candy:` list inside a named box body:

```yaml
kind-host:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-kind:v2026.269.1131'
```

```bash
charly deploy add kind-host --target local   # install kind on the host
```

A `kindcluster:` template then provisions a cluster (engine / `node_image` /
`nodes`); see `/charly-kubernetes:kubernetes`.

## Layout

- `charly.yml` — the `kind:` candy entity (the `KIND_VERSION` var, the
  `kind-engine-prepare` script step, the `check:` assertions, and the embedded
  `kind-skill:` skill entity).
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-kubernetes:kind` — the `kind` CLI, the
  `kindcluster:` substrate, and the per-engine preconditions.
- `/charly-kubernetes:check-k8s` — the `kube:` cluster-probe check verb.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
