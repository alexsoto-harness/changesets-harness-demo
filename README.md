# changesets-harness-demo

A small pnpm monorepo that reproduces the [`changesets/action`](https://github.com/changesets/action)
two-state behavior on **Harness CI** instead of GitHub Actions, then layers on governance
seams (security scan, approval gate, SBOM, registry-layer quarantine) that the stock GitHub
Action can't offer.

## Packages

| Package | Description |
|---|---|
| `@sotodemo/core` | Trivial placeholder library. |
| `@sotodemo/ui` | Depends on `@sotodemo/core` — demonstrates coordinated multi-package bumps. |

## The flow

```
push to main ──► [changesets-release pipeline]
                      │
                      ├─ changesets present?  ──► open/update "Version Packages" PR
                      │
                      └─ no changesets (PR merged) ──► pnpm release (changeset publish)
                                                       + create GitHub Release
```

## Local dev

```sh
pnpm install
pnpm changeset        # author a changeset
```

The registry is swappable at the pipeline layer (npm / JFrog / Harness AR) via two pipeline
variables and one secret — nothing registry-specific is committed here. Do **not** commit an
`.npmrc`; the pipeline writes one at runtime so the swap works.
