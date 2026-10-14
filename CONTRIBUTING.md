# Contributing to Gitify Renovate config

Thanks for helping maintain the organization's shared Renovate preset!

This repository holds the shared Renovate preset that every Gitify repository
inherits, plus the reusable validation workflow. See the [README][readme] for how
consumer repositories extend it.

## Proposing a change

1. Clone the repository and create a branch.
2. Edit [`default.json`][default-preset] (or `renovate.json` for this
   repository's own config).
3. Validate your change locally (see below).
4. Open a pull request describing the behavior change for consumers.

Use [conventional commit][conventional-commits] titles (`chore(deps):`, `feat:`,
`fix:`) so release and changelog automation keeps working.

## Validating locally

Run the same validator CI uses:

```sh
npx --yes --ignore-scripts --package renovate -- renovate-config-validator --strict default.json
```

Pass every file you changed. The `--strict` flag makes the validator fail on
warnings, matching the required check. The validator runs in CI on every pull
request, so a passing local run should match the `Validate Renovate config`
status check. CI pins a specific Renovate version; a newer local Renovate can
report rules the pinned version does not.

## What stays in the README

How a repository extends the preset, and how it calls the reusable validation
workflow, stays in the [README][readme] because it belongs to consumers of this
repository rather than contributors to it.

## Review and release

Changes are reviewed by the [code owners][codeowners] (`@setchy`,
`@afonsojramos`). There is nothing to publish: merging to `main` updates the
preset for every repository the next time Renovate runs.

<!-- LINK LABELS -->
[readme]: ./README.md
[default-preset]: ./default.json
[codeowners]: ./.github/CODEOWNERS
[conventional-commits]: https://www.conventionalcommits.org
