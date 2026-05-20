# bench/

Raw materials behind the benchmark in the [parent README](../README.md):
Ansible playbooks we ran and the logs they produced.

```
install/      snapshotter + containerd per node
convert/      one-time image conversion (Nydus / SOCI / eStargz)
test/         time pod-ready and first app metric
orchestrate/  install → convert → test in one shot
results/      raw logs, grouped by image and snapshotter
```

## Pod-ready (seconds, lower is better)

| Image                       | Registry          | OCI | Nydus  | SOCI | Stargz |
|-----------------------------|-------------------|----:|-------:|-----:|-------:|
| Jupyter PyTorch (3.6 GB)    | Harbor (internal) | 113 | **18** |  23  |   16   |
| Jupyter PyTorch (3.6 GB)    | External cloud    | 133 |   43   |  n/a |  110   |
| vLLM (8.3 GB)               | Harbor (internal) | 350 | **210**| 290  |  350   |
| vLLM (8.3 GB)               | External cloud    | 350 |  340   |  n/a |  360   |

Per-run logs in `results/<image>/<snapshotter>/`. External-cloud SOCI runs
are missing because the registry didn't expose the index format SOCI needs.

## Running it

Playbooks are published as reference — placeholders (`harbor.example.com`,
`myproject`, `CHANGE_ME`) must be adapted before they will run. Each file
documents its own prereqs in a leading comment. `orchestrate/` chains the
full install → convert → test flow.

Apache-2.0 — see [`LICENSE`](../LICENSE).
