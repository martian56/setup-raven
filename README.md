# setup-raven

A GitHub Action that installs the [Raven](https://github.com/martian56/raven)
toolchain (`raven`, `rvpm`) on a runner. It downloads a release, puts the
binaries on `PATH`, and caches the rvpm package cache between runs.

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: martian56/setup-raven@v1
    with:
      raven-version: "2.22.0"

  - run: rvpm build
  - run: rvpm test
  - run: rvpm fmt --check
```

With no `raven-version`, the latest release is installed:

```yaml
  - uses: martian56/setup-raven@v1
```

## Inputs

| Input | Default | Meaning |
|-------|---------|---------|
| `raven-version` | `latest` | Release to install, for example `2.22.0` |
| `cache` | `true` | Cache `~/.rvpm/cache`, keyed on the repository's `rv.toml` files |
| `token` | `github.token` | Token used to download the release assets |

## Outputs

| Output | Meaning |
|--------|---------|
| `raven-version` | The release tag that was installed, for example `v2.22.0` |

## Platforms

Linux (`x86_64-unknown-linux-gnu`) and Windows (`x86_64-pc-windows-msvc`),
matching the assets Raven releases publish. macOS fails with a clear error
until darwin assets exist.

## Caching

`rvpm` keeps fetched packages in `~/.rvpm/cache`, keyed by repository and
version, so the cache stays valid across Raven upgrades. The action restores
and saves it with `actions/cache`, keyed on the hash of every `rv.toml` in
the repository, with a fallback restore on the OS alone (stale extra
packages are harmless; rvpm resolves by exact version).

## License

MIT
