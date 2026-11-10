# tryAGI.Jint

Unmodified builds of [sebastienros/jint](https://github.com/sebastienros/jint), republished to nuget.org as
[`tryAGI.Jint`](https://www.nuget.org/packages/tryAGI.Jint).

## Why

Jint 5 previews are published only to upstream's feedz.io feed, which keeps the newest builds and deletes older
ones. A project that pins a preview stops restoring within days. nuget.org never deletes a version, so pinning
`tryAGI.Jint` keeps restoring.

## What is in the package

- Upstream source at a specific commit, built without changes. Only the **package ID** differs.
- Assembly name `Jint`, namespace `Jint` and upstream's strong-name key are unchanged, so switching is a
  `PackageReference` change and nothing else.
- The `[JsAccessible]` source generator (`Jint.SourceGenerators.Interop`) ships in the package as upstream packs it.
- Version: `5.0.0-preview.<upstream commit count>`. The package's repository commit metadata names the exact
  upstream commit.

## How it tracks upstream

[`publish.yml`](.github/workflows/publish.yml) runs nightly and on demand. It checks out upstream `main` (or a ref
given to `workflow_dispatch`), runs upstream's tests, packs, verifies the package contents and pushes. A version is
published only when upstream `main` has moved. Publishing uses nuget.org trusted publishing (OIDC); the repository
holds no API key.

## Trusted publishing policy

The workflow holds no API key; it exchanges a GitHub OIDC token for a short-lived nuget.org key. That exchange
only works if a matching policy exists on nuget.org, and nothing in this repository can create it. Without one the
run fails with `Token exchange failed (HTTP 401) ... No matching trust policy owned by user '<user>' was found`,
which is why `publish.yml` checks for it before the test and pack steps rather than at the push.

Create the policy at [nuget.org/account/trustedpublishing](https://www.nuget.org/account/trustedpublishing),
signed in as the **creator** user, with exactly these values:

| Field | Value |
|-------|-------|
| Policy creator (signed-in user) | `havendv` — this is what `NuGet/login` sends as `user`, overridable with the `NUGET_USER` repository variable. It is the account that creates the policy, not the package owner organization. |
| Package owner | The nuget.org account that owns `tryAGI.Jint` |
| Package | `tryAGI.Jint`, entered as a pattern — it cannot be picked from a list until a first version exists |
| Repository owner | `tryAGI` |
| Repository | `Jint` |
| Workflow file | `publish.yml` — the policy matches the caller workflow's filename, which is why `id-token: write` and `NuGet/login` must stay in this file and not move into a reusable workflow |
| Environment | Empty; the `publish` job declares no environment |

## License

Jint is BSD-2-Clause licensed by Sébastien Ros and contributors; see upstream's
[LICENSE.txt](https://github.com/sebastienros/jint/blob/main/LICENSE.txt). Packages carry upstream's
`BSD-2-Clause` license expression.
