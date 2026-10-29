<div align="center">

# Gitify Renovate config

**Shared [Renovate][renovate] presets & validation workflow** for the [Gitify organization][gitify-org].

[![Renovate config validation][validator-badge]][validator-actions] [![License][license-badge]][license]

</div>

> [!NOTE]
> Public repository. It hosts the organization's shared Renovate preset, which every Gitify repository inherits. Organization [community health files][github-health] live in [`gitify-app/.github`][dot-github].

## 🔧 Usage

Inherit the baseline preset from a repository's `renovate.json`:

```jsonc
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>gitify-app/renovate-config"],
  "packageRules": [
    { "description": "repo-specific rule" }
  ]
}
```

Repository-level settings and later `packageRules` take precedence over the shared baseline.

Because this repository follows Renovate's shared-preset naming convention, **new repositories are onboarded onto the preset automatically** — Renovate checks `gitify-app/renovate-config/default.json` before any other org default and suggests `extends: ["local>gitify-app/renovate-config"]`.

### ✅ Validating a repo's config

Consumer repositories can call the reusable workflow to validate their own `renovate.json`:

```yaml
jobs:
  validate:
    permissions:
      contents: read
    uses: gitify-app/renovate-config/.github/workflows/renovate-config-validator.yml@main
```

This trusted internal validator intentionally tracks `main`, so fixes reach
consumers without a digest-update PR. Consumers currently using a commit SHA must
replace that reference with `main`; the preset's `pinDigests: false` rule prevents
Renovate from pinning it again but does not rewrite existing SHA references.
Repositories running zizmor must also configure an explicit `ref-pin` policy
in `zizmor.yml`, preserving their other policies:

```yaml
rules:
  unpinned-uses:
    config:
      policies:
        "*": hash-pin
        "gitify-app/renovate-config/.github/workflows/renovate-config-validator.yml": ref-pin
```

The exception key must include the complete reusable workflow path; a bare
repository key does not match it. Keep this exception limited to this workflow,
not a repository-wide or organization-wide pattern.

Tracking a branch trusts future changes merged into this repository rather than
an immutable reviewed commit. This exception is only for the internal validator:
all third-party actions and workflows remain subject to the baseline's digest
pinning. Renovate matches reusable workflows by repository, so the exact
`gitify-app/renovate-config` package rule also applies to any future workflows in
this repository; do not broaden it to `gitify-app/**` or `setchy/**`.

## ⚖️ Policy

- 🗓️ Weekly updates, with grouped Vite+, GitHub Actions, Node.js runtime, and `@types/node` updates.
- 🟢 Node.js runtime (`engines`, `devEngines`, `.nvmrc`, `.node-version`, Docker images) and `@types/node` updates are grouped into one PR; Node runtime tracks LTS only.
- 🛡️ OSV vulnerability alerts on; no automerge in the baseline.
- 📌 npm dependency ranges pinned by default, with `engines` left as ranges.
- 📌 GitHub Actions digest pinning stays enabled except for the trusted internal Renovate validator described above.

## 🗃️ Presets

| File           | Purpose                                                                     |
| -------------- | --------------------------------------------------------------------------- |
| `default.json` | Baseline preset every repository inherits via `local>gitify-app/renovate-config`. |

## 📄 License

Released under the [MIT License](LICENSE).

---

<div align="center">

<sub>Maintained by <a href="https://github.com/orgs/gitify-app/teams/gitify-core">Gitify Core</a> · <a href="https://gitify.io">gitify.io</a></sub>

</div>

<!-- LINK LABELS -->
[renovate]: https://renovatebot.com/
[gitify-org]: https://github.com/gitify-app
[dot-github]: https://github.com/gitify-app/.github
[github-health]: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file
[validator-actions]: https://github.com/gitify-app/renovate-config/actions/workflows/renovate-config-validator.yml
[validator-badge]: https://img.shields.io/github/actions/workflow/status/gitify-app/renovate-config/renovate-config-validator.yml?logo=github&label=renovate%20config
[license]: LICENSE
[license-badge]: https://img.shields.io/badge/license-MIT-blue.svg
