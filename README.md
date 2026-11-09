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

## License

Jint is BSD-2-Clause licensed by Sébastien Ros and contributors; see upstream's
[LICENSE.txt](https://github.com/sebastienros/jint/blob/main/LICENSE.txt). Packages carry upstream's
`BSD-2-Clause` license expression.
