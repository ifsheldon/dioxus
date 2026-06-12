# Fork automation

This fork keeps two small workflows that are intentionally separate from the
upstream Dioxus release pipeline.

## Sync upstream

`sync-upstream.yml` runs every 15 minutes and can also be dispatched manually.
It merges `DioxusLabs/dioxus:main` into this fork's `main` branch and pushes the
result. It uses a normal merge, not a force push, so fork-only workflow commits
remain on top of upstream history. If upstream introduces a conflict with fork
files, the workflow fails and requires a manual merge.

## Release musl CLI

`release-musl-cli.yml` polls upstream stable releases twice per hour and can be
dispatched manually with a tag such as `v0.7.9`. It ignores drafts and
prereleases, fetches the matching upstream tag, builds `dioxus-cli` for
`x86_64-unknown-linux-musl`, and publishes these assets to the fork's matching
release:

- `dx-x86_64-unknown-linux-musl.zip`
- `dx-x86_64-unknown-linux-musl.tar.gz`
- `dx-x86_64-unknown-linux-musl.sha256`

The workflow skips work when all three assets already exist on the fork release.
Repository Actions settings must allow `GITHUB_TOKEN` read/write contents access
so the workflows can push tags, push `main`, create releases, and upload assets.
