# Releasing

Publishing is automated via GitHub Actions (`.github/workflows/publish.yml`) — there is no manual `npm publish`.

The npm token is **not** a per-repo secret — CI fetches it at publish time from a scoped, read-only secrets broker. The repo holds only two GitHub secrets: `NPM_TOKEN_URL` (the broker endpoint) and `NPM_BROKER_SECRET` (its credential).

## One-time setup

Set `NPM_TOKEN_URL` and `NPM_BROKER_SECRET` as repository secrets (rotate the token in the broker, never here). npm provenance also needs the repo + package public.

## Cut a release

1. Bump `version` in `package.json` (semver) and commit.
2. Tag + release — the tag must match the version:
   ```bash
   gh release create v0.1.0 --title v0.1.0 --generate-notes
   ```
3. The `publish` workflow builds (`tsc`) and runs `npm publish --provenance --access public`.

`workflow_dispatch` is also enabled, so you can re-run a publish manually from the Actions tab if needed.
