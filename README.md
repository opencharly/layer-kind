# layer-kind

The `layer-kind` candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, kind-prefixed
naming). The candy manifest lives at the repo root; the charly resolver fetches
this repo at the pinned tag.

It installs the pinned [`kind`](https://kind.sigs.k8s.io/) (Kubernetes-in-Docker)
tool plus a `kind-engine-prepare` script that applies the per-engine host
preconditions for kind's node containers (docker / rootless podman / rootless
nerdctl). Engine selection is charly's own engine word, mapped 1:1 to kind's
`KIND_EXPERIMENTAL_PROVIDER`.
